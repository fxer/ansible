# Secure a CentOS 7 server with ansible
This is my preferred way of locking down a new CentOS 7 VPS


Requirements
---
ansible host:
* ansible >= 2.0

target hosts:
* CentOS 7 (includes SELinux support)

Roles & Tasks
---
* common
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
  
* tor-relay
  * Install Tor
  * Configure SELinux
  * Configure firewalld
  * Create torrc config
  
## Examples Plays
Run the 'webservers' playbook against a specific host
* `ansible-playbook webservers.yml --limit HOST`

Run the 'tor-conf' task to rebuild a torrc files for every tor relay
* `ansible-playbook tor-relays.yml --tags "tor-conf"`