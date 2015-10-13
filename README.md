# Cyrus-Imapd

Cyrus-Imapd is a mail server.

It stores mailboxes and offers an access since any client which use IMAP/POP protocol

This playbook lets you install a simple instance of cyrus-imapd. It behaves like a backend IMAP.
You can install a complexe IMAP infrastructure with multiple backend IMAP. This is possible with Cyrus-murder concept.

Indeed, the main goal is about to have one aggregator (called murder server) and multiples backends IMAP. You can have multiple slaves aggregator (called frontend IMAP)
Each backend has its own mailboxes database. The master aggregates them, and the slaves pick up and use them to find where the mailboxes are stored. (on which backend)

## Installation

* Clone the repository
```
git clone https://github.com/recid/cyrus-imapd.git
```

* Get the latest version of Ansible
```
git clone git://github.com/ansible/ansible.git --recursive
cd ansible
source hacking/env-setup
```

* Get virtualenvwrapper

See the following link : http://virtualenvwrapper.readthedocs.org/en/latest/install.html

* Prepare ansible environment
```
mkvirtualenv -p /usr/bin/python2 --no-site-packages cyrus-imapd
pip install paramiko PyYAML Jinja2 httplib2 six pyasn1 pycrypto python-keyczar
cat > ~/.virtualenvs/cyrus-imapd/bin/postactivate << EOF
#!/bin/bash
cd $(pwd)
source ansible/hacking/env-setup
EOF
workon cyrus-imapd
```

## Playbook Configuration

In the config.yml file, you can indicate
 - Prerequiste System
 - LDAP informations,
 - SASL informations, 
 - Cyrus-Imapd informations,
 - enable or not local DNS records

### Prerequiste System
You can define the EPEL URLs and enable distribution packages upgrade

### LDAP informations
In this section, you can indicate the LDAP directory informations on which users and usersystem will authenticate.


### SASL informations
In this section, you can indicate the SASL backend (ldap or pam) and the SASL seach filter. SASL is used to the authentication


### Cyrus configuration
In this section you can indicate, TLS parameters, if you enable or not POP protocol and the murder informations.

### Local DNS
In this section, you can indicate the local DNS records.

## Inventory file
If you want to install a simple backend IMAP, you have to indicate the hostname in the cyrusfullservers group.

If you want to install a complexe IMAP architecteure, you have to indicate one and only one hostname in the cyrusmupdateservers group. Then you can indicate all hostname in cyrusbackservers and cyrusfrontservers



## Playbook Running
```
ansible-playbook -i inventory cyrus.yml
```
