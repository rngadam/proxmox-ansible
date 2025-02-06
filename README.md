## configure ansible

ensure latest community.general:

```
ansible-galaxy collection list | grep community.general
ansible-galaxy collection install --force community.general
```

edit ansible.cfg as necessary

## bootstrap

generate key:

```
ssh-keygen -f ~/.ssh/proxmox-ansible
```

create ansible user, setup password-less sudo:

```
ansible-playbook pve_onboard.yml -i inventory --user=root -k
```

testing

```
$ ansible pvenodes -m ping -i inventory --user=ansible  --private-key ~/.ssh/proxmox-ansible
192.168.2.226 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.11"
    },
    "changed": false,
    "ping": "pong"
}
```

## getting api keys

create API keys for user ansible and store in Ansible vault:

```
ansible-vault encrypt proxmox.enc
```

## setup proxmoxer

```
ansible-playbook pve_install_proxmoxer.yml -i inventory --user=ansible --private-key ~/.ssh/proxmox-ansible
```

## creating a vm

```
ansible-playbook  -i inventory -e @proxmox-rngadam.enc --vault-password-file password.pwd --user=ansible --private-key ~/.ssh/proxmox-ansible pve_create_vm.yml
```

## creating nixos

```
ansible-playbook  -i inventory -e @proxmox-root.enc --vault-password-file password.pwd --user=ansible --private-key ~/.ssh/proxmox-ansible pve_create_nixos.yml
```

## docs

* https://docs.ansible.com/ansible/latest/collections/community/general/proxmox_template_module.html
* https://mtlynch.io/notes/nixos-proxmox/
* https://hydra.nixos.org/build/283416299
* https://docs.ansible.com/ansible/latest/collections/community/general/proxmox_module.html