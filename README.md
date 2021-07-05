# Fortigate-CICD
this is a infrastructure as code implementation using Fortigate + Ansible + Azure devops pipline.
## how do i need?
for this implementation the following things are needed:

- a azure devops acount 
- a azure keyvault
- a ubuntu 20.04 server(can be hosted in azure or localy)
- credentials for the fortigate firewall

## how do i set this up?
for the setup we use a few steps to configure everything.


### azure keyvault 
first we start with the azure keyvault , in the keyvault we store the creentails that will be used te access the fortigate.  
the setup is simple. createe a new resource group or use an existing.  
then name the keyvault.
![alt text](https://github.com/bryanster/Fortigate-CICD/blob/main/docs/Pictures/keyvault1.png)

  
the correct rights needed for the pipline to read the passwords wil be added when we setup the pipeline
for now choose *review + create*

### setting up the project azure devops
i like to have 1 project containing all the managed network repositories
once the project is created i prefer to turn of all the unneeded settings in the project.   
under project settings turn off test plans and artifacts these wont be needed.
under the pipline settings we add a agent pool.  
for security purposes this is a self hosted type pool.  

clone this repository to use as a template

### setup Pipeline agent
start by installing a Ubuntu 20.04 server after install.
after installing ubuntu 20.04 we wil install the pipline agent.  
under the agent pool go to new agent there we wil install a linux agent X64.     
ssh into your server  
copy the link form download agent to your clipboard.  
on the ssh session run the command dont forgot to replace the < url > for the copied url.

        wget < url >

this wil download the package on the agent install page(devops):  
![alt text](https://github.com/bryanster/Fortigate-CICD/blob/main/docs/Pictures/newagent.png)
