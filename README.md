# DevOps-Case-Study-Part-2
 
This repository will act as the final capstone project for the Per Scholas 2020 Cloud DevOps program. Any documentation provided here has a specific purpose to meet the goals and requirements for this project set by Per Scholas and are not indicative of my usual documentation style, but will likely be quite similar. 

I wil cosnider this project a portfolio piece, but may change the name of the repository to indicate that later on.

Additionally, this project is a local deployment of flaks app running on a kubernetes cluster. Meaning it does not utitlize cloud providers such as AWS or GCP. Upon completion of this "local" project I will redo the project with a cloud provider.

---
## Environment

### Two VMs

jenkins-master: This VM was created manually through VirtualBox far ahead of this project's creation date. It serves as my all-purpose jenkins machine and will almost always be where my jenkins is run from. I'd like to briefly elaborate on why that is. Given that Jenkins will be repsonsible for running all my pipelines and has a GUI, I would prefer not to host it on a cloud provider as not only would that rack up a large bill whenever I want to do some small project, it is also resource inefficient as my local machine has plenbty of available resources to have Jenkins running basically whenever I am working. I also have opted to NOT dockerize my jenkins as I do not see a large benefit from messing with volume mounts and port fowarding. My jenkins VM only has tools necessary for a jenkisn pipeline (like Ansible or Terraform) and I add packages whenever I need them. Given that this is not a production environment I would prefer to not have to drift from a dockerfile that creates a jenkins container by installing new packages manually, or constantly be rebuilding an image. Bottom line, I prefer having jenkins run just on a VM.
- Base Image: ubutnu-server-20.04-amd-64
- Provider: VirtualBox
- Provisioner: Manual

case-study-worker: This is another ubuntu-server based VM and will act as my kubernetes control node.
- Base Image: peru/ubuntu-20.04-server-amd64 (Obtiained through Vagrant registry)
- Provider: VirtualBox
- Provisioner: Vagrant [Vagrantfile Link](./vagrant/Vagrantfile)

## Stage 1: Setup worker VM
The first step, besides documentation, is to provision a VM for our ansible to target, install necessary packages, and apply a deployemtn and service to a cluster, but that will coem later.

For now we simply navigate to the directory with the aformentioned Vagrantfile and run the following command 
```
vagrant up
```
Which will provision the virtual machine, and install our kubernetes cluster provider. Here is a snippet from my vagrantfile that show the pakcages to install upon creation
```
config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt install snapd -y
    snap install microk8s --classic
    apt install net-tools -y
  SHELL
```
Microk8s is our cluster provider and requires the snapd package to install.

Vagrant will handle all the heavy lifting and should install all packages mentioned above.

Some manual configuration is required after this step, such as changing the default password. It is also helpful to run an `ifconfig` and note the fresh VMs IP address.

We can now START the Microk8s cluster by running two simple commands

## Stage 2: Create Jenkinsfile
Because most, if not all, our configurations are being done through an ansible playbook and roles, all we need jenkins to do is call the playbook. 

While this projects Jenkinsfile is not at all complex, they can quickly become complex when the pipeline calls for things such as testing, which calls for a new environment (such as a docker container) to be created and the application (our flask app) to be run against test cases.

For this application there are no test cases, so we do not need to include this step.

## Stage 3: Setup Microk8s (Note: This is the most basic overview of the necessary steps and is not a replacement for following documentation)

Make sure the Microk8s service is running with
```
microk8s start
```

Then enable the DNS addon which we will need to access the service we will set up soon
```
microk8s enable dns
```

These steps are covered in the file `./ansible/microk8s/task/main.yml` as an ansible playbook