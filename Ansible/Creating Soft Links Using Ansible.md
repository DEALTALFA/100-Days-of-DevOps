The Nautilus DevOps team is practicing some of the Ansible modules and creating and testing different Ansible playbooks to accomplish tasks. Recently they started testing an Ansible file module to create soft links on all app servers. Below you can find more details about it.



Write a playbook.yml under /home/thor/ansible directory on jump host, an inventory file is already present under /home/thor/ansible directory on jump host itself. Using this playbook accomplish below given tasks:


Create an empty file /opt/dba/blog.txt on app server 1; its user owner and group owner should be tony. Create a symbolic link of source path /opt/dba to destination /var/www/html.


Create an empty file /opt/dba/story.txt on app server 2; its user owner and group owner should be steve. Create a symbolic link of source path /opt/dba to destination /var/www/html.


Create an empty file /opt/dba/media.txt on app server 3; its user owner and group owner should be banner. Create a symbolic link of source path /opt/dba to destination /var/www/html.
```
#inventory
stapp01 ansible_host=stapp01 ansible_ssh_pass=Ir0nM@n ansible_user=tony
stapp02 ansible_host=stapp02 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=stapp03 ansible_ssh_pass=BigGr33n ansible_user=banner
```
Here is an example of how the playbook.yml file can be structured to accomplish the tasks mentioned:


```yaml
---
- name: Create files and symbolic links on app servers
  hosts: all
  become: yes
  tasks:
    # Task for App Server 1
    - name: Create blog.txt on app server 1
      ansible.builtin.file:
        path: /opt/dba/blog.txt
        state: touch
        owner: tony
        group: tony
    ## or use ansible_host =='stapp01' if you have declared in inverntory you should not use double curly braces {{ }} inside a when statement. The when clause is already evaluated as a Jinja2 expression by default.
      when: inventory_hostname == 'stapp01'

    # Task for App Server 2
    - name: Create story.txt on app server 2
      ansible.builtin.file:
        path: /opt/dba/story.txt
        state: touch
        owner: steve
        group: steve
      when: inventory_hostname == 'stapp02'

    # Task for App Server 3
    - name: Create media.txt on app server 3
      ansible.builtin.file:
        path: /opt/dba/media.txt
        state: touch
        owner: banner
        group: banner
      when: inventory_hostname == 'stapp03'

    # Common task for all app servers
    - name: Create symbolic link from /opt/dba to /var/www/html
      ansible.builtin.file:
        src: /opt/dba
        dest: /var/www/html
        state: link
        force: yes