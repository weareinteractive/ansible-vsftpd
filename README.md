# Ansible franklinkim.vsftpd role

[![Build Status](https://img.shields.io/travis/weareinteractive/ansible-vsftpd.svg)](https://travis-ci.org/weareinteractive/ansible-vsftpd)
[![Galaxy](http://img.shields.io/badge/galaxy-franklinkim.sudo-blue.svg)](https://galaxy.ansible.com/list#/roles/1974)
[![GitHub Tags](https://img.shields.io/github/tag/weareinteractive/ansible-vsftpd.svg)](https://github.com/weareinteractive/ansible-vsftpd)
[![GitHub Stars](https://img.shields.io/github/stars/weareinteractive/ansible-vsftpd.svg)](https://github.com/weareinteractive/ansible-vsftpd)

> `franklinkim.vsftpd` is an [Ansible](http://www.ansible.com) role which:
>
> * installs vsftpd
> * configures vsftpd
> * manages user

## Installation

Using `ansible-galaxy`:

```shell
$ ansible-galaxy install franklinkim.vsftpd
```

Using `requirements.yml`:

```yaml
- src: franklinkim.vsftpd
```

Using `git`:

```shell
$ git clone https://github.com/weareinteractive/ansible-vsftpd.git franklinkim.vsftpd
```

## Dependencies

* Ansible >= 2.4
* franklinkim.openssl

## Variables

Here is a list of all the default variables for this role, which are also available in `defaults/main.yml`.

```yaml
---
#
# vsftpd_users:
#   - username: ftpuser
#     name: FTP User
#     # openssl passwd -salt 'somesalt' -1 'secret'
#     password: '$1$somesalt$jezmI5TSY7mVTzHLgsK5L.'
# vsftpd_config:
#   local_umask: 022
# vsftpd_seboolean:
#    ftp_home_dir: yes
#    ftpd_full_access: no
#

# define package (version)
vsftpd_package: vsftpd
# users to create with nologin
vsftpd_users: []
# user group to create with nologin
vsftpd_group: ftp
# start on boot
vsftpd_service_enabled: yes
# current state: started, stopped
vsftpd_service_state: started
# default ssl key
vsftpd_key_file: ssl-cert-snakeoil.key
# default ssl cert
vsftpd_cert_file: ssl-cert-snakeoil.pem
# config variables
vsftpd_config: {}
# config template to install, relative to the ansible repository root
vsftpd_config_template:
# optional SELinux booleans to be set i.e. for RedHat
vsftpd_seboolean: {}

```

## Handlers

These are the handlers that are defined in `handlers/main.yml`.

```yaml
---

- name: restart vsftpd
  service:
    name: vsftpd
    state: restarted
  when: vsftpd_service_state != 'stopped'

```


## Usage

This is an example playbook:

```yaml
---

- hosts: all
  roles:
    - franklinkim.vsftpd
  vars:
    vsftpd_users:
       - username: ftpuser
         name: FTP User
         # python -c 'import crypt; print crypt.crypt("ftpuser", "$1$SomeSalt$")'
         password: '$1$SomeSalt$Mabm7JORLYoZUVe5er5Ss.'
    vsftpd_config:
      log_ftp_protocol: YES
      local_enable: YES
      write_enable: YES
      xferlog_enable: YES
      listen_port: 990

```


## Testing

```shell
$ git clone https://github.com/weareinteractive/ansible-vsftpd.git
$ cd ansible-vsftpd
$ make test
```

## Contributing
In lieu of a formal style guide, take care to maintain the existing coding style. Add unit tests and examples for any new or changed functionality.

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

*Note: To update the `README.md` file please install and run `ansible-role`:*

```shell
$ gem install ansible-role
$ ansible-role docgen
```

## License
Copyright (c) We Are Interactive under the MIT license.
