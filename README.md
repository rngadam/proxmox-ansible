## configure ansible

ensure latest community.general:

```
ansible-galaxy collection list | grep community.general
ansible-galaxy collection install --force community.general
```

## bootstrap

generate key:

```
ssh-keygen -f ~/.ssh/proxmox-ansible
```

create ansible user, setup password-less sudo:

```
ansible-playbook pve_onboard.yml -i inventory --user=root -k
```

## configure ansible defaults

edit ansible.cfg as necessary to set reasonable defaults

## testing

```
$ ansible pvenodes -m ping
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
ansible-playbook pve_install_proxmoxer.yml
```

## creating a vm

```
ansible-playbook -e @proxmox-rngadam.enc  pve_create_vm.yml
```

## creating nixos

```
ansible-playbook  -i inventory -e @proxmox-root.enc pve_create_nixos.yml
```

## docs

* https://docs.ansible.com/ansible/latest/collections/community/general/proxmox_template_module.html
* https://mtlynch.io/notes/nixos-proxmox/
* https://hydra.nixos.org/build/283416299
* https://docs.ansible.com/ansible/latest/collections/community/general/proxmox_module.html
* https://github.com/UntouchedWagons/Ubuntu-CloudInit-Docs