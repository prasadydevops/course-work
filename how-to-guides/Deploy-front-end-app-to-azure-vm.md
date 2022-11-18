# Deploy static web app to azure VM

## Prerequisites

- A VM Created in azure portal. Follow [this guide](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/quick-create-portal) if you need help with that.

> Make sure to allow ports ssh (22), http (80), https (443) for incoming traffic.
> Have access to VM via ssh

## Steps

### Configuring VM as web server

- Login to as a standard user with access to `sudo` group.

  `ssh azureuser@<vm_ip>`

- Create a empty folder in home directory that will contain the web app build.

  `mkdir -p ~/app`

#### Install and configure nginx

- Install nginx

  `sudo apt install nginx`

> After this step visit http://<vm_ip> and make sure you are seeing nginx default website.

#### Add ssl certificate with let's enrcypt

- Add DNS records for your vm in your domain provider site. (eg: godaddy)

For SSL Certificate generation please follow [Certbot Instructions | Certbot](https://certbot.eff.org/instructions?ws=nginx&os=ubuntufocal) for this step.

> After this step application should run at `https://domain-name.com`

### Pipeline to publish static build to a VM

#### Create a ssh service connection.

- Create a service ssh connection following this path, make sure to put correct vi IP and private key which has access to VM.
  Org name / Project name / Settings / Service connections

#### Create pipeline for CI & CD


- Create `azure-pipelines.yml` file with below content in project root directory with correct service ssh connection name created in previous step.
- Azure will use this to automatically build & deploy the app whenever there is a new commit push to `main` branch.


```
# Starter pipeline
# Start with a minimal pipeline that you can customize to build and deploy your code.
# Add steps that build, run tests, deploy, and more:
# https://aka.ms/yaml

trigger:
  - main

pool:
  vmImage: ubuntu-latest

steps:
  # Install nodejs
  - task: NodeTool@0
    inputs:
      versionSpec: "16.x"

  # Run node_modules installation and build command
  - script: npm install && npm run build
    displayName: "Building project"

  # Publish artifacts
  - task: PublishBuildArtifacts@1
    inputs:
      PathtoPublish: "$(Build.ArtifactStagingDirectory)"
      ArtifactName: "drop"
      publishLocation: "Container"

  # Deploy: Copy files to VM over ssh
  - task: CopyFilesOverSSH@0
    inputs:
      <!-- sshEndpoint: "vm-lms-az1-sc" -->
      sshEndPoint: "<service_ssh_connection_created_in_prev_step>"
      contents: "**"
      targetFolder: "/home/azureuser/app"
      readyTimeout: "20000"

```
