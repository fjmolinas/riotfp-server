# riot-fp server ansible

### `common` role

Sets up the following:

- System upgrade (*including kernel*)
- Adding some useful packages (*curl, vim, tmux, git…*)
- UFW firewall rules allowing user-specified ports and protocols
- SSH with key-only authentication
- Install docker

- Optional custom SSH banner
- Optionally add a list of user to the sudoers with NOPASSWD

### Usage

First update the `hosts` file to target your server. An example is
provided in: `host.inc`, so you can simply:

```shell
cp hosts.inc inventories/hosts
```

and modify the ip address or domain name if you have one.

Second create a `authorized_keys` file in `roles/common/files` with the
list
of authorized ssh-keys.

Then the first time run (next times --ask-become-pass can be ignored)"

```shell
ansible-playbook -i inventories/hosts playbook.yml -u <user> --ask-become-pass
```

**You can also store user name in inventory file and user's pass in your
Ansible vault.**

### Assumptions

A default user `admin` is used and needs sudo privilege. If none is set use the
`user_account.yml` playbook to create one, this needs to run as
`root` user.

```shell
ansible-playbook -i inventories/hosts playbook.yml -u root --ask-pass
```

### Deploy SSL certificates

Use ansible VAULT to create or edit certificates and place
it in `vars/`:

```
# Create a new vault
ansible-vault create certificate.yml

# Edit an existing vault
ansible-vault edit certificate.yml
```

Now add the certificate and private keys:

```
---
ssl_certificate: |
  -----BEGIN CERTIFICATE-----
  ...
  -----END CERTIFICATE-----

ssl_private_key: |
  -----BEGIN PRIVATE KEY-----
  ...
  -----END PRIVATE KEY-----
```

Then install by:

```
ansible-playbook -i inventories/hosts ssl_install.yml --ask-vault-pass
```
