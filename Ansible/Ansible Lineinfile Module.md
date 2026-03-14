The Nautilus DevOps team want to install and set up a simple httpd web server on all app servers in Stratos DC. They also want to deploy a sample web page using Ansible. Therefore, write the required playbook to complete this task as per details mentioned below.


We already have an inventory file under /home/thor/ansible directory on jump host. Write a playbook playbook.yml under /home/thor/ansible directory on jump host itself. Using the playbook perform below given tasks:


Install httpd web server on all app servers, and make sure its service is up and running.


Create a file /var/www/html/index.html with content:


This is a Nautilus sample file, created using Ansible!


Using lineinfile Ansible module add some more content in /var/www/html/index.html file. Below is the content:

Welcome to Nautilus Group!


Also make sure this new line is added at the top of the file.


The /var/www/html/index.html file's user and group owner should be apache on all app servers.


The /var/www/html/index.html file's permissions should be 0655 on all app servers.

```yaml
- hosts: all
  become: true
  tasks:
    - name: "Install httpd on server"
      package:
        name: "httpd"
        state: "present"
    - name: "start httpd"
      service:
        name: "httpd"
        state: "started"
        enabled: "yes"
    - name: "Add content to file"
      copy:
        content: |
          This is a Nautilus sample file, created using Ansible!
        dest: "/var/www/html/index.html"
        owner: "apache"
        group: "apache"
        mode: "0655"
    - name: "Add line at the top of the file"
      lineinfile:
        line: "Welcome to Nautilus Group!"
        group: "apache"
        owner: "apache"
        mode: "0655"
        dest: "/var/www/html/index.html"
        state: present
        insertafter: BOF
    - name: "Check"
      command: cat /var/www/html/index.html
      register: command_output
    - debug:
        msg: "{{ command_output.stdout_lines }}"
```

