# Provision a CentOS 7 server with Ansible
This is my preferred way of provisioning and locking down a new CentOS 7 VPS

## Requirements
`ansible-galaxy install -r requirements.yml`

## Roles & Tasks

### Dependencies
* [fxer.ansible-common](https://github.com/fxer/ansible-common)
* [fxer.ansible-nginx](https://github.com/fxer/ansible-nginx)
* [fxer.ansible-appserver](https://github.com/fxer/ansible-appserver)

### tor-relay
* Install Tor
* Configure SELinux
* Configure firewalld
* Create torrc config

## Example Plays

Run the 'appservers' playbook against a specific host

```
ansible-playbook appservers.yml --ask-become-pass --limit HOST
```

Run the 'tor-conf' task to rebuild the torrc files for every tor relay

```
ansible-playbook tor-relays.yml --ask-become-pass --tags tor-conf
```

## License
2-clause BSD
