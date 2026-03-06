One of the DevOps team members has created a zip archive on jump host in Stratos DC that needs to be extracted and copied over to all app servers in Stratos DC itself. Because this is a routine task, the Nautilus DevOps team has suggested automating it. We can use Ansible since we have been using it for other automation tasks. Below you can find more details about the task:


We have an inventory file under /home/thor/ansible directory on jump host, which should have all the app servers added already.


There is a zip archive /usr/src/data/nautilus.zip on jump host.


Create a playbook.yml under /home/thor/ansible/ directory on jump host itself to perform the below given tasks.


Unzip /usr/src/data/nautilus.zip archive in /opt/data/ location on all app servers.


Make sure the extracted data must has the respective sudo user as their user and group owner, i.e tony for app server 1, steve for app server 2, banner for app server 3.


The extracted data permissions must be 0755.

```yml
- hosts: all
  become: true
  tasks:
    - name: "Unarchive the file"
      unarchive:
        src: "/usr/src/data/nautilus.zip"
        dest: "/opt/data"
        mode: "0755"
        owner: "{{ ansible_user }}"
        group: "{{ ansible_user }}"
```