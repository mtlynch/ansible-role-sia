# Ansible Role: Sia

[![Build Status](https://travis-ci.org/mtlynch/ansible-role-sia.svg?branch=master)](https://travis-ci.org/mtlynch/ansible-role-sia)
[![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-sia-blue.svg?style=flat-square)](https://galaxy.ansible.com/mtlynch/sia)
[![License](http://img.shields.io/:license-mit-blue.svg?style=flat-square)](LICENSE)

The Sia Ansible role installs [Sia](https://sia.tech/) on a node and automates its initial provisioning steps.

## Features

* Unattended install
  * This role allows you to provision a Sia node from start to finish completely unattended. No more waiting hours for the blockchain to sync or the wallet to unlock before you can proceed to the next step of installation.
* Service installation
  * Will install Sia as a systemd service.
* Idempotent
  * This role is idempotent. You can run it against the same server multiple times and it will not change the server's state unless necessary.
* Simple upgrade
  * With this role, upgrading your Sia version is simply a matter of changing the version variable.

## Role Variables

Available variables are listed below, along with default values (see [defaults/main.yml](defaults/main.yml)):

The version of Sia to install:

```yaml
sia_version: 1.3.7
```

The location to install Sia:

```yaml
sia_dir: "/opt/{{ sia_package }}"
```

The convenience symlink that will always point to the latest version of Sia (e.g. `/opt/sia/siac wallet`):

```yaml
sia_symlink: "/opt/sia"
```

The location to store Sia data files (blockchain files, encrypted wallet files, and other Sia metadata):

```yaml
sia_data_dir: "/opt/sia-data"
```

Specifies whether or not to use the [bootstrap method](https://siawiki.tech/daemon/bootstrapping_the_blockchain) to sync the Sia blockchain:

```yaml
sia_bootstrap_blockchain: False
```

Username and group to use for Sia service:

```yaml
sia_user: sia
sia_group: "{{ sia_user }}"
```

Sia modules to load (see the `--modules` parameter of [siad](https://github.com/NebulousLabs/Sia/blob/master/cmd/siad/main.go)):

```yaml
sia_modules: cgtwrh
```

Path (on the control node, localhost) to a file to store newly created seed passphrases. If specified, and ansible-role-sia provisions a node with an unitialized wallet, Ansible will initialize the wallet with a random Sia seed and save the passphrase to this file. If the node already has an initialized wallet, this variable is ignored.

```yaml
sia_seed_path: null
```

The 29-word Sia passphrase to use to initialize the node's Sia wallet. If the node already has an initialized wallet, this variable is ignored.

```yaml
sia_seed: null
```

The passphrase to decrypt the node's wallet:

```yaml
sia_wallet_password: "{{ sia_seed }}"
```

## Dependencies

None

## Example Playbook

#### `example.yml`

```yaml
- hosts: all
  roles:
    - { role: mtlynch.sia }
```

### Running Example Playbook

To run the example playbook, `example.yml` (above), run the following commands:

```shell
ansible-galaxy install mtlynch.sia
ansible-playbook example.yml
```

### Additional Examples

See [this blog post](https://blog.spaceduck.io/automating-sia-setup/) for additional examples.

## License

MIT

## Author Information

This role was created in 2018 by [Michael Lynch](https://mtlynch.io).
