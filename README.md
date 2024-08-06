# Table of Contents

* [Base ssh configuration](#ansible)
* [Install&&Check](#install)
* [Upgrade packages](#update)
* [Playbook](#playbook)

## Ansible

### <a id="ansibles">Base ssh configuration</a>

```sh
First connect

1 ---> clone VM machine (example 1 - master, 2 - worker servers)
2 ---> sudo apt install/upgrade openssh-server
3 ---> fingerprint master to worker (ssh x.x.x.x)

Generate key-pair

1 ---> cd .ssh
2 ---> ssh-keygen -t ed25519 -C '<NAME>'
example "id_ed25519" "id_ed25519.pub -> public"
3 ---> ssh-copy-id -i ~/.ssh/<NAME>.pub <IP_WORKER>

Ansible-key

1 ---> ssh-keygen -t ed25519 -C 'ansible'
2 ---> :/home/amizory/.ssh/ansible
3 ---> ssh-copy-id -i ~/.ssh/ansible.pub x.x.x.x
4 ---> CONNECT -> ssh -i ~/.ssh/ansible x.x.x.x
```

### <a id="install">Install&&Check</a>

```sh
1 ---> sudo apt upgrade && sudo apt install ansible
2 ---> [master] -> nano inventory --> add worker ip
3 ---> [master] -> ansible all --key-file ~/.ssh/ansible -i inventory.ini -m ping
4 ---> [master] -> nano ansible.cfg 

A)  [defaults]
    inventory = inventory
    private_key_file = ~/.ssh/ansible

        -> ansible all -m ping`

B)  [defaults]
    inventory = inventory
    private_key_file = ~/.ssh/ansible
    module_name = ping
    pattern = all

        -> ansible all


5 ---> ansible all --list-host
6 ---> ansible --version
7 ---> ansible all -m gather_facts --limit <WORKER_IP> ---> all info 
```

### <a id="update">Upgrade packages</a>

```sh
ansible all 
            -m apt                          --> module
            -a "name=tmux state=latest"     --> process  
            --become-method=sudo            --> privilege
            --become-user=root              --> user
            --ask-become-pass               --> pass
            -vvv                            --> info -f

ansible all 
            -m shell

            -a "apt update && apt upgrade -y && apt autoremove -y && apt clean -y"
                updates-cache/updates-packages/deletes-old-packages/clears-cache
or 
            -a "apt dist-upgrade -y" 
                all these actions together + updating dependencies

            --become-method=sudo 
            --become-user=root 
            --ask-become-pass 
```

### <a id="playbook">Playbook</a>

```yaml
- name: First playbook
  hosts: myhosts
  tasks:
   - name: Ping my hosts
     ansible.builtin.ping:

   - name: Print message
     ansible.builtin.debug:
      msg: Hello world

   - name: Update 
     ansible.builtin.apt:
      update_cache: true

   - name: Install nginx
     ansible.builtin.apt:
      name: "nginx"
      state: present/latest/absent
```

```sh
ansible-playbook -i inventory.ini <FILE>.yaml
```
