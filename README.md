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

after creating the keyvault add a access policy under the access policy pane create a new access policy select secret managment:  
![alt text](https://github.com/bryanster/Fortigate-CICD/blob/main/docs/Pictures/keyvault2.png)

select principal en kies voor de Repository  

![alt text](https://github.com/bryanster/Fortigate-CICD/blob/main/docs/Pictures/keyvault3.png)
 
add the policy dont forget to save the policy:

![alt text](https://github.com/bryanster/Fortigate-CICD/blob/main/docs/Pictures/keyvault4.png)  


### setting up the project azure devops
i like to have 1 project containing all the managed network repositories
once the project is created i prefer to turn of all the unneeded settings in the project.   
under project settings turn off test plans and artifacts these wont be needed.
under the pipline settings we add a agent pool.  
for security purposes this is a self hosted type pool.  

clone this repository to use as a template

### setup Pipeline agent
start by installing a Ubuntu 20.04 server after install.
this is a default install ontop of that you need te install ansible on the host i recommend following the ansible documentation:

[LINK](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-on-ubuntu)  
after installing ubuntu 20.04 we wil install the pipline agent.  
under the agent pool go to new agent there we wil install a linux agent X64.     
ssh into your server  
copy the link form download agent to your clipboard.  
on the ssh session run the command dont forgot to replace the < url > for the copied url.

        wget < url >

this wil download the package on the agent install page(devops):  
![alt text](https://github.com/bryanster/Fortigate-CICD/blob/main/docs/Pictures/newagent.png)

on the configure.sh
accept the license
for the server url use : https://dev.azure.com/< yourorg name >
in my case it is: https://dev.azure.com/bryan0110/

for the personal access token you need te generate one everything
![alt text](https://github.com/bryanster/Fortigate-CICD/blob/main/docs/Pictures/pat.png)

![alt text](https://github.com/bryanster/Fortigate-CICD/blob/main/docs/Pictures/agent.png)
give the name of you agent pool you created earlier;

use the default working directory: _work

start the agent a

        ./run.sh


under the agent pool it should be online:

![alt text](https://github.com/bryanster/Fortigate-CICD/blob/main/docs/Pictures/agentpool.png)


### add credentials to the keyvault
for security purposes i add an new acount for de Azure pipline with trusted hosts restricted to the azure pipline agent


![alt text](https://github.com/bryanster/Fortigate-CICD/blob/main/docs/Pictures/fgtuser.png)

by doing this you shield the acount from being used other then by the pipeline

we then add the password to the Azure keyvault add tthem as a secret to the

![alt text](https://github.com/bryanster/Fortigate-CICD/blob/main/docs/Pictures/secret.png)

### azure pipline configuration
for the pipline config you can copy most of the Yaml file but you need te re-add your keyvault start by removing the keyvault part:

```
- task: AzureKeyVault@1
  inputs:
    azureSubscription: 'yeet'
    KeyVaultName: 'fortitools'
    RunAsPreJob: true
    
```
can be removed from the yaml file  
then add an azure keyvault using the assistent:  

![alt text](https://github.com/bryanster/Fortigate-CICD/blob/main/docs/Pictures/pipeline1.png)

authorize for the subscription your keyvault is under:  

![alt text](https://github.com/bryanster/Fortigate-CICD/blob/main/docs/Pictures/pipeline2.png)

select your keyvault you can keep te filter on *:


![alt text](https://github.com/bryanster/Fortigate-CICD/blob/main/docs/Pictures/pipeline3.png)

move the keyvault part to the fromt of the tasks.
after re-adding the keyvault make sure your pool name is set to the agent pool created earlier.

Change the ansible password variable to the name of your secret in the keyvault.  
![alt text](https://github.com/bryanster/Fortigate-CICD/blob/main/docs/Pictures/pipeline4.png)  

save the pipline but dont run it yet.
for the pipline to function you should change the ip in the hosts file to your fortigate.   
![alt text](https://github.com/bryanster/Fortigate-CICD/blob/main/docs/Pictures/host.png)

run the pipline for the first time it will give an message that additional acces is needed press view then permit this access  
![alt text](https://github.com/bryanster/Fortigate-CICD/blob/main/docs/Pictures/pipeline5.png)  


run the pipeline again and it should work.


## how does it work?
azure devops triggers the pipline for you everytime you make a change to the Main/Master branch the pipline will rollout the ansible playbook over the host in in host file the ansible uses the Rest api of fortigate to do all the configuration as designed in the playbooks taskt