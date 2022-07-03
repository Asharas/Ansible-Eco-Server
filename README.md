Ansible Eco Server
=========

This role aims at deploying a self-hosted [Eco](https://play.eco/) server.  


Requirements
------------

[Jeff Geerling](https://www.jeffgeerling.com/)'s [ansible-role-certbot](https://galaxy.ansible.com/geerlingguy/certbot), see [Dependencies](#Dependencies).  
If you didn't install the Eco server role from Ansible Galaxy, run the following command to get the Certbot role:  
```
ansible-galaxy install geerlingguy.certbot
```  

Role Variables
--------------  
Variables are split into three files:  
- [default/main/main.yml](default/main/main.yml)   
- [default/main/eco.yml](default/main/eco.yml)  
- [vars/main.yml](vars/main.yml)  

##### [default/main/main.yml](default/main/main.yml)  
Role specific variables.


| Variable | Description | Default |
| --- | --- | --- |
| eco_fqdn | Eco server Fully Qualified Domain Name | `''` |
| eco_iptables | Set iptables rules | `false` |
| eco_revproxy | Enable nginx as reverse proxy | `false` |
| eco_revproxy_tls | Enable nginx tls config | `false` |
| eco_revproxy_tls_selfsigned | Generate a self-signed X509 certificate | `false` |
| eco_revproxy_certbot | Enable use of [ansible-role-certbot](https://galaxy.ansible.com/geerlingguy/certbot), see [Dependencies](#Dependencies) | `false` |
| eco_revproxy_discord_redirect | Add a redirection `/discord` to Discord server URL | `false` |
| eco_skip_restart | Do not restart the server after playing the role even if changes occur | `false` |  

##### [default/main/eco.yml](default/main/eco.yml)
Game specific variables.  
Those are too numerous to detail and doing so would be redundant. Take a look at the [default/main/eco.yml](default/main/eco.yml) file, [Eco server configuration](https://wiki.play.eco/en/Server_Configuration) and [Eco text formatting](https://nodecraft.com/support/games/eco/adding-formatting-and-colors-to-the-server-name-for-your-eco-server).  

Quick note on `eco_remote_address` and `eco_web_url`: the former gets assigned `eco_fqdn` value and the latter is templated depending on whether TLS is enabled or not. Both should be left as is.

##### [vars/main.yml](vars/main.yml)  
Both role and game variables that most installs don't need modified.  


Dependencies
------------

Certbot is enabled by setting `eco_revproxy_certbot: true` and needs `eco_fqdn` and `certbot_admin_email` for the certificate to be generated.  
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
    - role: eco_server
```

For more configuration options check the roles's [sources on GitHub](https://github.com/geerlingguy/ansible-role-certbot).  


Example Playbook
----------------
##### Bare minimum
```yaml
---
- hosts: eco
  gather_facts: yes
  become: yes

  roles:
    - role: eco_server
```  

##### Custom difficulty  
To set your own difficulty, override the eco_difficulty_presets

```yaml  
---
- hosts: eco
  gather_facts: yes
  become: yes

  vars:
    eco_difficulty: "custom"
    eco_difficulty_stacksize_mod: 1.0
    eco_difficulty_weight_mod: 1.0
    eco_difficulty_fuel_mod: 1.0
    eco_difficulty_conrange_mod: 1.0
    eco_difficulty_presets:
      # eco_difficulty_presets.custom does not exists by default, it needs to be fully set in order to be used
      custom:
        name: "Custom"
        endgame_cost: "Normal"
        diff_mods:
          craft_resource_mod: 0.5
          craft_time_mod: 0.5
          player_xp_per_spec_xp: 0.0
          skill_gain_multiplier: 5
          spec_cost_multiplier: 0.1
          spec_xp_per_level: 25.0

  roles:
    - role: eco_server
```
Notes
-----

###### Configuration templating  
Most configuration is bulk templated by looping on `eco_generate_conf` except for the following configuration files:  
- `Pause.eco`: Templated only if it doesn't exist.  
- `Network.eco`: Special treatment because of `ID` and `Passport` game variables generated at world creation.  
- `Maintenance.eco`: Optional conf, templated only if `eco_autoshutdown_hour != -1̀`.  

More refactoring is considered as the bulk generation tasks are poorly optimized, see next note on idempotency.  

###### Idempotency
This role isn't perfectly idempotent as it does restart the game server on the first run after world generation even if no changes to vars are made.  
This is because the game server has a static ordering of JSON objects that it overwrites on each modification whereas the way the configs are merged sorts entries alphabetically.   
`Network.eco` as already been refactored to avoid this but not sure it'll be doable for every config file, especially `WorldGenerator.eco` which is massive.

###### Upcoming or considered features  

- Re-add mods support and more mods:  
  - Discord Link](https://github.com/Eco-DiscordLink/EcoDiscordPlugin)  
  - Big Shovel](https://eco.mod.io/big-shovel)
  - Colored Vehicules](https://eco.mod.io/colored-vehicles)  

License
-------

 <p xmlns:cc="http://creativecommons.org/ns#" xmlns:dct="http://purl.org/dc/terms/"><a property="dct:title" rel="cc:attributionURL" href="https://gitlab.com/Asharas/ansible-eco-server">Ansible Eco Server</a> by <a rel="cc:attributionURL dct:creator" property="cc:attributionName" href="https://gitlab.com/Asharas">Asharas</a> is licensed under <a href="http://creativecommons.org/licenses/by-nc-sa/4.0/?ref=chooser-v1" target="_blank" rel="license noopener noreferrer" style="display:inline-block;">CC BY-NC-SA 4.0<img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/cc.svg?ref=chooser-v1"><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/by.svg?ref=chooser-v1"><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/nc.svg?ref=chooser-v1"><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/sa.svg?ref=chooser-v1"></a></p>

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
