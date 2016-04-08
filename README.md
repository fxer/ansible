# Provision a CentOS 7 server with Ansible
This is my preferred way of provisioning and locking down a new CentOS 7 VPS

Requirements
---
* **host** Ansible >= 2.0
* **servers** CentOS 7 (includes SELinux support)

Roles & Tasks
---
### common
* Enable SELinux (immediate reboot to activate)
* Add swap (otherwise a small VPS may be visited by the OOM Killer in later tasks)
* Enable firewalld
* Create initial users (ssh keys, passwords & sudo access)
* Harden sshd configuration
  * Disable root login
  * Disable password authentication
  * Move to non-default port (also handles SELinux & firewalld config for this)
* Update all existing packages
* Install user-defined base packages (ex: vim, unzip)
* Enable auto-updates (via yum-cron)
* Install global shell enhancements (custom aliases, vim configs)
* Set timezone
 
### appserver
* Install official nginx repo (stable or mainline branch)
	* Install latest nginx
	* Install vim syntax highlighting
	* Install custom Diffie-Hellman parameters
	* Install supplemental configs (ex: php-fpm)
	* Configure an optimized nginx.conf
	* Configure server blocks for dictionary of individual sites
* Create systemd .service files for dictionary of individual apps

### tor-relay
* Install Tor
* Configure SELinux
* Configure firewalld
* Create torrc config
  
Example Plays
---
Run the 'appservers' playbook against a specific host

```
ansible-playbook appservers.yml --ask-become-pass --limit HOST
```

Run the 'tor-conf' task to rebuild the torrc files for every tor relay

```
ansible-playbook tor-relays.yml --ask-become-pass --tags tor-conf
```