# Server Setup Playbook for Ansible

## Prerequisites

* Python
* Ansible (either from pip or Homebrew)
* passlib (from pip)

## Configuration

### First Time Configuration

Create the password hash for the user to configure:

```shell
python -c "from passlib.hash import sha512_crypt; import getpass; print sha512_crypt.using(rounds=5000).hash(getpass.getpass())"
```

Then update the hosts.yml with the user, password, SSH port (if needed), and the hosts to configure:

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

### Re-Playing on Already Configured Hosts

Since the playbook will disable root login and change the SSH port, the main thing here is to remember to change those settings for the hosts that have already been configured.

```yaml
all:
  vars:
    user: chris
    password: password hash goes here
    public_key: ~/.ssh/id_rsa.pub
    ssh_port: 12022
  hosts:
    "10.0.0.1":
      ansible_user: chris
      ansible_port: 12022
      ansible_become: yes
      ansible_become_method: sudo
```

When running the playbook this way, be sure to specify the `-K` flag as this will prompt you for the `sudo` password.

## Running

Normal, first-time use:

```shell
ansible-playbook ./server-setup.yml
```

Updating hosts using `sudo`:

```shell
ansible-playbook -K ./server-setup.yml
```