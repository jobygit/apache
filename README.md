# apache
Ansible playbook to install apache in  Redhat and Debian Hosts

---
- name: "Installing Apache On Both Debian And Redhat"
  hosts: all
  become: yes
  tasks:
    
    - name: "Debain - Installing Apache"
      when: ansible_os_family == "Debian"
      apt:
        name: apache2
        state: present
        cache_update: true
            
    - name: "Debian - Service Starting And Enabling"
      when: ansible_os_family == "Debian"
      service:
        name: apache2
        state: started
        enabled: true
            
    - name: "Debian - Creating index.html file"
      when: ansible_os_family == "Debian"
      copy:
        content: "<h1><center>Debian Sample Page</center></h1>"
        dest: /var/www/html/index.html
        owner: apache2
        group: apache2
            
    - name: "RedHat - Installing Apache"
      yum:
        name: httpd
        state: present
      when: ansible_os_family == "RedHat"
            
    - name: "RedHat - Service Starting And Enabling"
      service:
        name: httpd
        state: started
        enabled: true
      when: ansible_os_family == "RedHat"
        
        
    - name: "RedHat - Creating index.html file"
      copy:
        content: "<h1><center>Redhat Sample Page</center></h1>"
        dest: /var/www/html/index.html
        owner: apache
        group: apache
      when: ansible_os_family == "RedHat"
