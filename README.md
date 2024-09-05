Lighthouse
=========

This role can install and configure [Lighthouse](https://github.com/VKCOM/lighthouse.git) on Centos 8

Requirements
------------

You neeed to install and configure `git` and `NGINX` (or another web server) before run this role.

Role Variables
--------------

| vars | Description | Value | Location |
|------|------------|---|---|
| lighthouse_dir | Where to store Lighthouse files | "/home/{{ ansible_user_id }}/lighthouse" | defaults/main.yml |
| lighthouse_link | URL of Clickhouse repo | "https://github.com/VKCOM/lighthouse/archive/refs/heads/master.zip" | vars/main.yml |

Example Playbook
----------------

```yml
- name: Install Lighthouse
  tags: lighthouse
  hosts: lighthouse
  handlers:
    - name: Nginx reload
      become: true
      ansible.builtin.service:
        name: nginx
        state: restarted
  pre_tasks:
    - name: Copy nginx configuration file
      become: true
      ansible.builtin.template:
        src: nginx_lighthouse.conf
        dest: "/etc/nginx/conf.d/lighthouse.conf"
        owner: "0"
        group: "0"
        mode: "0664"
      notify: Start nginx service
  roles:
    - lighthouse
```

License
-------

MIT

Author Information
------------------

Aleksandr Egorkin
