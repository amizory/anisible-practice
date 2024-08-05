# Table of Contents

* [Base ssh configuration](#ansible)

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
