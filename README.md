# ansible network automation
for an automated install of fortigate config i build this ansible [playbook](https://github.com/bryanster/fortigate_ansible/blob/master/fortigate.yaml)  
  
  
requirements:

- ansible 2.9
- fortinet.fortios

on the install of the fortigate galaxy check [versioning](https://ansible-galaxy-fortios-docs.readthedocs.io/en/latest/version.html)
so you use the correct version for you fortios version
my playbook are build on 6.2.5 and is tested working  
this is a pretty simple configuration to serve as an example
this configuration is pushed using an azure pipeline
