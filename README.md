# Ansible Password Changer Playbook

## 1. install ansible

```
$ sudo apt-get update
$ sudo apt-get install software-properties-common
$ sudo apt-add-repository ppa:ansible/ansible
$ sudo apt-get update
$ sudo apt-get install ansible
```

## 2. add hosts to hosts file (host)

```
[all]
<ip address 1> ansible_ssh_port=22 ansible_user=<privileged user>
<ip address 2> ansible_ssh_port=22 ansible_user=<privileged user>
...
<ip address n> ansible_ssh_port=22 ansible_user=<privileged user>
```

## 3. create playbook (changepwd.yml)

```
- name: Change user password
  hosts:  all
  vars_files:
    - ./config.yml
  tasks:
  - name: Changing password
    become: true
    user:
      name: "{{ user.username }}"
      update_password: always
      password: "{{ user.password }}"
```

## 4. create config file (config.yml)

```
user:
  username: <username>
  password: <password>
```

Note: Hashed password must be used. Generate password by using `openssl passwd -1 <password>`.

## 5. Run ansible-playbook
```
$ ansible-playbook -i hosts changepwd.yml --ask-become-pass
SUDO password:
```