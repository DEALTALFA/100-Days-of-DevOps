There is some data on all app servers in Stratos DC. The Nautilus development team shared some requirement with the DevOps team to alter some of the data as per recent changes they made. The DevOps team is working to prepare an Ansible playbook to accomplish the same. Below you can find more details about the task.


Write a playbook.yml under /home/thor/ansible on jump host, an inventory is already present under /home/thor/ansible directory on Jump host itself. Perform below given tasks using this playbook:


We have a file /opt/devops/blog.txt on app server 1. Using Ansible replace module replace string xFusionCorp to Nautilus in that file.


We have a file /opt/devops/story.txt on app server 2. Using Ansiblereplace module replace the string Nautilus to KodeKloud in that file.


We have a file /opt/devops/media.txt on app server 3. Using Ansible replace module replace string KodeKloud to xFusionCorp Industries in that file.
```yaml
---
- name: Replace strings in files on app servers
  hosts: all
  become: yes
  tasks:
    - name: Replace xFusionCorp with Nautilus in blog.txt on app server 1
      replace:
        path: /opt/devops/blog.txt
        regexp: 'xFusionCorp'
        replace: 'Nautilus'
      when: inventory_hostname == 'stapp01'
    - name: Replace Nautilus with KodeKloud in story.txt on app server 2
      replace:
        path: /opt/devops/story.txt
        regexp: 'Nautilus'
        replace: 'KodeKloud'
      when: inventory_hostname == 'stapp02'
    - name: Replace KodeKloud with xFusionCorp Industries in media.txt on app server 3
      replace:
        path: /opt/devops/media.txt
        regexp: 'KodeKloud'
        replace: 'xFusionCorp Industries'
      when: inventory_hostname == 'stapp03'
```