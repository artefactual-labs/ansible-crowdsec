# Crowdsec
This ansible roles installs Crowdsec incl. hub, collections, scenarios, postoverflows, parsers, bouncers and prometheus endpoint.

## Requirements
Ansible master running version 2.12 

Tested on:
```yaml
  platforms:
    - name: Ubuntu
      versions:
        - bionic  #18.04 LTS
        - focal   #20.04 LTS
        - impish  #21.10
        - jammy   #22.04 LTS Not tested
    - name: Debian
      versions:
        - bookworm # 12
        - bullseye # 11
    - name: EL
      versions:
        - '9'   #Rocky
        - '8'   #Rocky & alma Linux og Oracle Linux
        - '7'   #Oracle Linux
```

## how to install.
I use ansible-galaxy do make a requirements.yml
```yaml
roles:
  - geerlingguy.security
  - alf149.crowdsec
```
And run 
`ansible-galaxy install -r requirements.yml` This wil import this role to your ansible projekt. 


## Role Variables
Available variables with default values (see `defaults/main.yml`)
variables can be host specific in group_vars/host.yml

## Example Playbook
```yaml
- hosts: all

  vars:
    cs_ban_duration: "duration: 4h" # PROD eg. 10m for testing

  roles:
    - alf149.crowdsec 
```

## Manual tasks could be handy
ansible HOST -m shell -a "sudo cscli parsers install crowdsecurity/whitelists --force"
ansible 'group' -m shell -a "sudo cscli parsers remove crowdsecurity/whitelists --force"
ansible 'group' -m shell -a "sudo systemctl reload crowdsec"

## Add bouncers to lapi server

You can get the crowdsec bouncer id from bouncers directory, i.e:

```
cat /etc/crowdsec/bouncers/crowdsec-firewall-bouncer.yaml.id
cs-firewall-bouncer-2715248097
```

And then add it to the lapi server list:

crowdsec_agent_bouncers:
  - cs-firewall-bouncer-2715248097

So it must be done after installing the bouncer.

## Use lapi server with external MySQL database

This role was originally created to use local psql database for lapi server. Now you can use 
an external MySQL database with:

```
crowdsec_lapi_db: mysql # Use mysql or psql
crowdsec_mysql_db_user: crowdsec
crowdsec_mysql_db_password: 'VeryLongPasswordPsqlChangeme2024!'
crowdsec_mysql_db_name: crowdsec
crowdsec_mysql_db_host: localhost
```

The role asumes the MySQL database is already configured and the access is granted.

## TODO
- Test on Windows server  
- Maby autodetect nftables/iptables and load the correct bouncer. 

## Error reporting. 
Use github issues or make a PR. 

## Author Information
------------------

[Alf149](https://github.com/alf149) 
