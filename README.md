Role Name  
=========

A brief description of the role goes here.

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------
See [server configuration]

## Network

| Var name | default value | Description |  
| --- | --- | --- |
| eco_game_port | 3000 | |  
| eco_web_port | 3001 | |  
| eco_rcon_port | 3002 | |  
| eco_rcon_password | "" | |  
| eco_remote_address | "" | ?? |  
| eco_relay | "" | This server will be used as fallback to proxy traffic between server and client if no direct connection possible. |  
| eco_web_url | "" | |  
| eco_is_public | "false" | |  
| eco_discord | "" | |  
| eco_upnp | "true" | |  
| eco_password | "" | |  
| eco_category | "None" | Server for public server listing within the brower. <br> Can be one of `None`, `Beginner`, `Established`, `Beginner Hard` or `Strange`|  
| eco_playtime | "" | |  
| eco_description | "Eco Server" | Server description, 500 char max. See [Formatting] |  
| eco_detailed_desc | "" | [Formatting] |  

[server configuration]: [https://wiki.play.eco/en/Server_Configuration]
[Formatting]: [https://nodecraft.com/support/games/eco/adding-formatting-and-colors-to-the-server-name-for-your-eco-server]

Dependencies
------------

This role calls [Jeff Geerling](https://www.jeffgeerling.com/)'s amazing [ansible-role-certbot](https://galaxy.ansible.com/geerlingguy/certbot).  
It is enabled by setting `eco_certbot: True` and needs `eco_fqdn` and `certbot_admin_email` to be set.  
You'll need a valid domain name for the certificate to be generated. Getting one is beyond the scope of this document.

```
---
- hosts: eco
  gather_facts: yes
  become: yes

  vars:
    eco_fqdn: eco.example.com
    # Set this var to true to request a staging certificate
    certbot_testmode: true
    certbot_admin_email: admin@example.com
```

For more configuration options check the roles's [sources on GitHub](https://github.com/geerlingguy/ansible-role-certbot).  


Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

 <p xmlns:cc="http://creativecommons.org/ns#" xmlns:dct="http://purl.org/dc/terms/"><a property="dct:title" rel="cc:attributionURL" href="https://gitlab.com/Asharas/ansible-eco-server">Ansible Eco Server</a> by <a rel="cc:attributionURL dct:creator" property="cc:attributionName" href="https://gitlab.com/Asharas">Asharas</a> is licensed under <a href="http://creativecommons.org/licenses/by-nc-sa/4.0/?ref=chooser-v1" target="_blank" rel="license noopener noreferrer" style="display:inline-block;">CC BY-NC-SA 4.0<img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/cc.svg?ref=chooser-v1"><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/by.svg?ref=chooser-v1"><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/nc.svg?ref=chooser-v1"><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/sa.svg?ref=chooser-v1"></a></p>

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
