Ansible playbook for Ruby on Rails hosts.

This allows [us](http://www.spacebabies.nl) to provision a VPS to run a Ruby on Rails application.

You will need [Ansible](http://docs.ansible.com/intro_installation.html) version 1.6 or later in order to continue. So, go ahead and install that on your local computer.

* ruby 2 compiled from source
* nginx packaged from Phusion
* PostgreSQL from ubuntu package
* Redis from ubuntu package

## How this works

We use this playbook as follows:

1. Clone the public git repo
2. Rename origin to upstream: `git remote rename origin upstream`
3. Configure the variables on a per-site basis (see below)
4. Add a new private repo: `git remote add origin git@repo.com`
5. Push to the origin: `git push -u origin master`
6. From time to time, pull in new changes from upstream: `git pull upstream master`

## Installation and configuration

To run the playbook, first install Ansible 1.6+ and put the hosts to be provisioned in a file named `hosts.ini` as such:

```
[rails]
www.example.com
```

Then copy `vars/defaults.yml` to `vars/user.yml` and customize with the actual values.

# https

Each virtualhost needs a corresponding TLS certificate. It must be generated locally and added to the repo. This way, the identical environment will be recreated on a fresh instance. Obviously the private key must be closely guarded, make sure it stays secure.

Generate a new key and Certificate Signing Request:

```
openssl req -nodes -newkey rsa:2048 -keyout roles/vhosts/files/$VIRTUALHOST.2014.key -out roles/vhosts/files/$VIRTUALHOST.2014.csr -subj "/C=NL/ST=Noord-Holland/L=Amsterdam/O=Space Babies B.V./OU=/CN=$VIRTUALHOST"
```

Generate a self-signed certificate to get started. Replace this with the actual, CA provided certificate.

```
openssl x509 -req -days 365 -in roles/vhosts/files/$VIRTUALHOST.2014.csr -signkey roles/vhosts/files/$VIRTUALHOST.2014.key -out roles/vhosts/files/$VIRTUALHOST.2014.cer
```

Commit the lot to git:

```
git commit roles/vhosts/files -am "generated key, csr and certificate"
```

## Passwords

Generate a few passwords on the commandline and put these in the following files:

1. `roles/postgresql/vars/main.yml`

A good password can be generated with `openssl rand -base64 12`.

## Provisioning

Deploy a new VPS instance running Ubuntu 14.04 and make sure a user `ubuntu` exists with sudo rights.

## Running

To run the entire playbook and provision/update the server:

```
ansible-playbook -i hosts.ini site.yml --ask-sudo-pass
```
