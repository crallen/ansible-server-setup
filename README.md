# Server Setup Playbook for Ansible

## Prerequisites

* Python
* Ansible (either from pip or Homebrew)
* passlib (from pip)

## First Time Configuration

1. Create the password hash for the user to configure:
   ```shell
   python -c "from passlib.hash import sha512_crypt; import getpass; print sha512_crypt.using(rounds=5000).hash(getpass.getpass())"
   ```

2. Update the hosts.yml with the user, password, SSH port (if needed), and the hosts to configure:
   ```yaml
   all:
     vars:
       user: chris
       password: password hash goes here
       public_key: ~/.ssh/id_rsa.pub
       ssh_port: 12022
     hosts:
       "10.0.0.1"
       "host.example.com"
   ```

## Re-Playing on Already Configured Hosts

