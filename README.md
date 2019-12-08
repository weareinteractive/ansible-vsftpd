# Ansible weareinteractive.vsftpd role

[![Build Status](https://img.shields.io/travis/weareinteractive/ansible-vsftpd.svg)](https://travis-ci.org/weareinteractive/ansible-vsftpd)
[![Galaxy](http://img.shields.io/badge/galaxy-weareinteractive.vsftpd-blue.svg)](https://galaxy.ansible.com/weareinteractive/vsftpd)
[![GitHub Tags](https://img.shields.io/github/tag/weareinteractive/ansible-vsftpd.svg)](https://github.com/weareinteractive/ansible-vsftpd)
[![GitHub Stars](https://img.shields.io/github/stars/weareinteractive/ansible-vsftpd.svg)](https://github.com/weareinteractive/ansible-vsftpd)

> `weareinteractive.vsftpd` is an [Ansible](http://www.ansible.com) role which:
>
> * installs vsftpd
> * configures vsftpd
> * manages user

**Note:**

> Since Ansible Galaxy supports [organization](https://www.ansible.com/blog/ansible-galaxy-2-release) now, this role has moved from `franklinkim.vsftpd` to `weareinteractive.vsftpd`!

## Installation

Using `ansible-galaxy`:

```shell
$ ansible-galaxy install weareinteractive.vsftpd
```

Using `requirements.yml`:

```yaml
- src: weareinteractive.vsftpd
```

Using `git`:

```shell
$ git clone https://github.com/weareinteractive/ansible-vsftpd.git weareinteractive.vsftpd
```

## Dependencies

* Ansible >= 2.4
* {"role"=>"weareinteractive.openssl", "when"=>["vsftpd_enable_ssl|default(true)"], "tags"=>["openssl-dependency", "dependencies", "openssl"]}

## Variables

Here is a list of all the default variables for this role, which are also available in `defaults/main.yml`.

```yaml
---
#
# vsftpd_users:
#   - username: ftpuser
#     name: FTP User
#     state: present (default) or absent
#     remove: yes or no (default)
#     comment: FTP User for file transfer
#     uid: 13370
#     home: /path/to/home
#     password: "{{ 'ftpuser' | password_hash('sha256', 'mysecretsalt') }}"
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
# enable ssl by default
vsftpd_enable_ssl: true
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
  # Ignore errors due to: https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=754762;msg=9
  ignore_errors: yes

```


## Usage

This is an example playbook:

```yaml
---

- hosts: all
  roles:
    - weareinteractive.vsftpd
  vars:
    vsftpd_users:
       - username: ftpuser
         name: FTP User
         password: "{{ 'ftpuser' | password_hash('sha256', 'mysecretsalt') }}"
    vsftpd_config:
      listen_port: 990
      local_enable: YES
      write_enable: YES
      chroot_local_user: YES
      xferlog_enable: YES
      log_ftp_protocol: YES
      allow_writeable_chroot: YES

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
