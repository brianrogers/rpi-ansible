Ansible Debian/Devuan bootstrap
===============================

[![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-HanXHX.debian_bootstrap-blue.svg)](https://galaxy.ansible.com/HanXHX/debian_bootstrap) [![Build Status](https://travis-ci.org/HanXHX/ansible-debian-bootstrap.svg?branch=master)](https://travis-ci.org/HanXHX/ansible-debian-bootstrap)

This role bootstraps Debian/Devuan server:

- Configure APT (sources.list)
- Install minimal packages (vim, htop...)
- Install Intel/AMD microcode if needed
- Install and configure Local DNS with [Unbound](https://www.unbound.net). Feature in beta-test! DO NOT USE IT IN PRODUCTION!
- Install and configure NTP daemon ([OpenNTPd](http://www.openntpd.org/) or [NTP](http://support.ntp.org/))
- Add groups, users with SSH key, sudoers
- Deploy bashrc, vimrc for root
- Update few alternatives
- Configure system: hostname, timezone and locale
- Purge, delete and avoid systemd if wanted
- Sysctl tuning


Requirements
------------

This role needs `sudo` package already installed.

Role Variables
--------------

### APT configuration

Theses variables define hostname to configure APT (normal repo and backports):

- `rpi_apt_default_host`: repository host. It can replace the last one (installed with this role) with a new one
- `rpi_apt_use_src`: install "deb-src" repositories (default: false)
- `rpi_apt_components`: components uses in sources.list (default: "main contrib non-free")

### Role setup

- `rpi_set_hostname`: if true, change hostname
- `rpi_set_locale`: if true, configure locales
- `rpi_set_timezone`: if true, set timezone
- `rpi_set_ntp`: if true, install and configure OpenNTPd

### System configuration

- `rpi_hostname`: system hostname
- `rpi_default_locale`: default system locale
- `rpi_locales`: list of installed locales
- `rpi_timezone`: system timezone
- `rpi_sysctl_config: list of kernel parameters, see`: [default/main.yml]
- `rpi_use_systemd`: delete systemd if set to false (persistent)
- `rpi_use_unbound`: configure Local DNS and manage network (default is false)
- `rpi_use_dotfiles`: overwrite root dotfiles (bashrc, screenrc, vimrc)
- `rpi_uninstall_packages`: packages list to uninstall

### Alternatives

- `rpi_alternative_editor`
- `rpi_alternative_awk`

### NTPd

- `rpi_ntp_hosts`: hostnames NTP server list
- `rpi_ntp_pkg`: package used to provide NTP: "openntpd" or "ntp"

### Group

- `rpi_groups`: list of group

Each row have few keys:

- `name`: (M) username on system
- `system`: (O) yes/no (default: no)
- `state`: (O) present/absent (default: present)

(M) Mandatory
(O) Optionnal

### User

- `rpi_users`: list of user

Each row have few keys:

- `name`: (M) username on system
- `shell`: (O) default is /bin/bash
- `comment`: (O) default is an empty string
- `sudo`: (O) boolean (true = can sudo)
- `group`: (O) main group (default is `name` without password)
- `groups`: (O) comma separated list of groups
- `createhome`: (O) yes/no
- `system`: (O) yes/no (default: no)
- `ssh_keys`: (M) list of ssh public keys. If you don't need any SSH key, please provide an empty list.
- `state`: (O) present/absent (default: present)

(M) Mandatory
(O) Optionnal

### Readonly vars

- `rpi_packages`: list of packages to install
- `rpi_hostname_files`: list of file where we should substitute bad hostname
- `rpi_microcode_apt_distribution`: location of package to install microcode
- `rpi_distro_packages`: list specific package to install (related to OS version)

Dependencies
------------

None.

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: HanXHX.debian_bootstrap }


About TravisCI
--------------

Due to Docker limitations, we can't check:

- Removing systemd
- Setting hostname
- Setting locales
- Configure unbound
- Configure sysctl


License
-------

GPLv2

Author Information
------------------

- Twitter: [@hanxhx_](https://twitter.com/hanxhx_)
