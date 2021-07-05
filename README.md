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
