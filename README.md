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

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
