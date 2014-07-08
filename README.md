Ansible playbook for Ruby on Rails hosts.

This allows me to provision a VPS to run a Ruby on Rails application.

* ruby 2 compiled from source
* nginx packaged from Phusion
* PostgreSQL from ubuntu package
* Redis from ubuntu package

## Installation and configuration

To run the playbook, first install Ansible 1.6+ and put the hosts to be provisioned in `./hosts` as such:

```
[rails]
hostname.example.com
```

## Running

To run the entire playbook and provision/update the server:

```
ansible-playbook -i hosts site.yml
```
