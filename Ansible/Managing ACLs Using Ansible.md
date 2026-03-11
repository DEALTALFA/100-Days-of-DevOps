There are some files that need to be created on all app servers in Stratos DC. The Nautilus DevOps team want these files to be owned by user root only however, they also want that the app specific user to have a set of permissions on these files. All tasks must be done using Ansible only, so they need to create a playbook. Below you can find more information about the task.



Create a playbook named playbook.yml under /home/thor/ansible directory on jump host, an inventory file is already present under /home/thor/ansible directory on Jump Server itself.


Create an empty file blog.txt under /opt/sysops/ directory on app server 1. Set some acl properties for this file. Using acl provide read '(r)' permissions to group tony (i.e entity is tony and etype is group).


Create an empty file story.txt under /opt/sysops/ directory on app server 2. Set some acl properties for this file. Using acl provide read + write '(rw)' permissions to user steve (i.e entity is steve and etype is user).


Create an empty file media.txt under /opt/sysops/ on app server 3. Set some acl properties for this file. Using acl provide read + write '(rw)' permissions to group banner (i.e entity is banner and etype is group).
```yml
---
- name: Create files with ACL permissions on app servers
  hosts: all
  become: yes
  tasks:
    - name: Create /opt/sysops directory
      file:
        path: /opt/sysops
        state: directory
        owner: root
        group: root
        mode: '0755'

    - name: Create blog.txt on app server 1 with ACL for group tony
      block:
        - name: Create empty blog.txt file
          file:
            path: /opt/sysops/blog.txt
            state: touch
            owner: root
            group: root
            mode: '0644'
        - name: Set ACL read permissions for group tony
          acl:
            path: /opt/sysops/blog.txt
            entity: tony
            etype: group
            permissions: r
            state: present
      when: inventory_hostname == 'stapp01'

    - name: Create story.txt on app server 2 with ACL for user steve
      block:
        - name: Create empty story.txt file
          file:
            path: /opt/sysops/story.txt
            state: touch
            owner: root
            group: root
            mode: '0644'
        - name: Set ACL read+write permissions for user steve
          acl:
            path: /opt/sysops/story.txt
            entity: steve
            etype: user
            permissions: rw
            state: present
      when: inventory_hostname == 'stapp02'

    - name: Create media.txt on app server 3 with ACL for group banner
      block:
        - name: Create empty media.txt file
          file:
            path: /opt/sysops/media.txt
            state: touch
            owner: root
            group: root
            mode: '0644'
        - name: Set ACL read+write permissions for group banner
          acl:
            path: /opt/sysops/media.txt
            entity: banner
            etype: group
            permissions: rw
            state: present
      when: inventory_hostname == 'stapp03' 

---
# Another clean way to do
- name: Configure specific file ACLs using variables
  hosts: all
  become: yes
  vars:
    # We define our data structure here
    sysops_files:
      - hostname: stapp01
        filename: blog.txt
        entity: tony
        etype: group
        permissions: r
      - hostname: stapp02
        filename: story.txt
        entity: steve
        etype: user
        permissions: rw
      - hostname: stapp03
        filename: media.txt
        entity: banner
        etype: group
        permissions: rw

  tasks:
    - name: Process files and ACLs
      block:
        - name: Create the file
          file:
            path: "/opt/sysops/{{ item.filename }}"
            state: touch
            owner: root
            group: root
            mode: '0644'

        - name: Apply ACL permissions
          ansible.posix.acl:
            path: "/opt/sysops/{{ item.filename }}"
            entity: "{{ item.entity }}"
            etype: "{{ item.etype }}"
            permissions: "{{ item.permissions }}"
            state: present
            
      # This loop runs the block for every item in our variable list
      loop: "{{ sysops_files }}"
      # The 'when' ensures the task only runs if the hostname matches
      when: inventory_hostname == item.hostname

```
Key Components Explained
- become: yes: This ensures the tasks are run with sudo privileges, which is required to change file ownership to root and modify ACLs.

- file module with state: touch: This creates the file if it doesn't exist without changing the content if it does.

- acl module: This is the standard way to handle Access Control Lists in Ansible. Unlike standard Linux permissions (UGO), ACLs allow you to grant specific permissions to users/groups that aren't the primary owner.
    - entity: The name of the user or group.

    - etype: Defines if the entity is a user or a group.

    - permissions: Use standard shorthand like r, w, or rw.

**Note**: Ensure that the acl package is installed on the target managed servers, as the Ansible acl module relies on the setfacl and getfacl binaries being present on the remote system.