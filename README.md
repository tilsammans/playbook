Ansible playbook for Ruby on Rails hosts.

This allows me to provision a VPS to run a Ruby on Rails application.

## Installation and configuration

To run the playbook, first install Ansible and put the hosts to be provisioned in `/etc/ansible/hosts` as such:

```
[rails]
hostname.example.com
```

You must place host variables in `/etc/ansible/host_vars/hostname.example.com`:

```
---
domain: example.com
ssl_subject: "/C=NL/ST=NH/L=Amsterdam/O=Acme, Inc/OU=L33t Haxxor Dept./CN=www.example.com"
```

## Running

To run the entire playbook and provision/update the server:

```
ansible-playbook site.yml
```

## Ruby setup

* ruby 2 compiled from source
* nginx packaged from Phusion
