Ansible Eco Server
=========

This role aims at deploying a self-hosted [Eco](https://play.eco/) server.  


Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

[defaults/main/main.yml](defaults/main/main.yml) contains most game related variables you might want to alter. See also [Eco server configuration](https://wiki.play.eco/en/Server_Configuration).

[Formatting]: [https://nodecraft.com/support/games/eco/adding-formatting-and-colors-to-the-server-name-for-your-eco-server]

Dependencies
------------

This role calls [Jeff Geerling](https://www.jeffgeerling.com/)'s amazing [ansible-role-certbot](https://galaxy.ansible.com/geerlingguy/certbot).  

If you didn't install the Eco server role from Ansible Galaxy, run the following command to get the Certbot role:  
```
ansible-galaxy install geerlingguy.certbot
```  

Certbot is enabled by setting `eco_certbot: True` and needs `eco_fqdn` and `certbot_admin_email` for the certificat to be generated.  
You'll need a valid domain name for the certificate to be generated. Getting one is beyond the scope of this document.

If you didn't install the Eco server role from Ansible Galaxy, run the following command to get the Certbot role:  
```
ansible-galaxy install geerlingguy.certbot
```  

Certbot is enabled by setting `eco_certbot: True` and needs `eco_fqdn` and `certbot_admin_email` for the certificat to be generated.  
You'll need a valid domain name for the certificate to be generated. Getting one is beyond the scope of this document.

```yaml
---
- hosts: eco
  gather_facts: yes
  become: yes

  vars:
    eco_fqdn: eco.example.com
    # Set this var to true to request a staging certificate
    certbot_testmode: true
    certbot_admin_email: admin@example.com


  roles:
    - role: ansible_eco_server
```

For more configuration options check the roles's [sources on GitHub](https://github.com/geerlingguy/ansible-role-certbot).  


Example Playbook
----------------

```yaml
---
- hosts: eco
  gather_facts: yes
  become: yes

  vars_files:
    - vars.yml

  roles:
    - role: ansible_eco_server
```  

Notes
-----

###### Idempotency
This role isn't perfectly idempotent as it does restart the game server on the first run after world generation even if no changes to vars are made.  
This is because the game server has a static ordering of JSON objects that it overwrites on each modification whereas the way the configs are merged sorts entries alphabetically.   
The configuration templating steps might be optimizable to improve this.  

###### Upcoming or considered features
Re-add mods support and more mods:
- [Big Shovel](https://eco.mod.io/big-shovel)
- [Colored Vehicules](https://eco.mod.io/colored-vehicles)  

License
-------

 <p xmlns:cc="http://creativecommons.org/ns#" xmlns:dct="http://purl.org/dc/terms/"><a property="dct:title" rel="cc:attributionURL" href="https://gitlab.com/Asharas/ansible-eco-server">Ansible Eco Server</a> by <a rel="cc:attributionURL dct:creator" property="cc:attributionName" href="https://gitlab.com/Asharas">Asharas</a> is licensed under <a href="http://creativecommons.org/licenses/by-nc-sa/4.0/?ref=chooser-v1" target="_blank" rel="license noopener noreferrer" style="display:inline-block;">CC BY-NC-SA 4.0<img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/cc.svg?ref=chooser-v1"><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/by.svg?ref=chooser-v1"><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/nc.svg?ref=chooser-v1"><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/sa.svg?ref=chooser-v1"></a></p>

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
