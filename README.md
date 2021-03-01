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
- Provisioner: Vagrant
