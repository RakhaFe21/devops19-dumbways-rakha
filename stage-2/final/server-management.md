# Server Management
**Requirements**

-   1 SSH keys max.
-   SSH Config.
-   Ubuntu 22.04 lts

**Instructions**

-   Create new user  `finaltask-$USER`
-   Server login only with SSH key
-   **Password login disabled**
-   Create a working  **SSH config**  to log into servers
-   Only use  **1 SSH keys**  for all purpose (Repository, CI/CD etc.)
-   UFW enabled with only used ports allowed
-   Change ssh port from (22) to (1234)

## Server Configuration using Ansible


1. Attach the SSH Key on every servers, use the same SSH Key
2. Adding the config ssh                                          
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2014-39-45.png?raw=true)
3. Configuration ansible environment
	- Hosts
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2014-09-46.png?raw=true)
	-  ansible.cfg
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/raw/main/stage-2/week-3/assets/Screenshot%20from%202024-01-14%2009-44-16.png?raw=true)
	-  group_vars/all
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2014-15-29.png?raw=true)
4. Create user ansible playbook
	```sh
	- hosts: all
	  become: true
	  tasks:
	
	  - name: "Add the new user for biznet server"
	    user:
	      name: '{{username}}'
	      password: "$6$ae9NqHtr3bZhkL8J$7c2oniGemVDGYYWBPRzTdOmegFg3pu1wCa15chUVypVaJIvFqRN6sU2G/OFsdvvUqtekVJ2iFAPJCxWWvCw0L." #pw=ansible123
	      shell: /bin/bash
	      groups: sudo
	      append: yes
	      home: /home/{{username}}
	      state: present
	
	  - name: "create .ssh folder"
	    file: 
	      path: ~{{username}}/.ssh
	      state: directory
	      owner: "{{username}}"
	      group: "{{username}}"
	      mode: 0700
	
	  - name: "create authorized_keys"
	    copy:
	      src: ~/.ssh/id_rsa.pub
	      dest: ~{{username}}/.ssh/authorized_keys
	      owner: "{{username}}"
	      group: "{{username}}"
	      mode: 0600
	  
	  - name: "Configuration PasswordAuthentication"
	    lineinfile:
	      path: '{{item}}'
	      regexp: 'PasswordAuthentication yes'
	      line: 'PasswordAuthentication no'
	    loop:
	      - /etc/ssh/sshd_config
	
	  - name: "Configuration PubkeyAuthentication"
	    lineinfile:
	      path: '{{item}}'
	      regexp: 'PubkeyAuthentication no'
	      line: 'PubkeyAuthentication yes'
	    loop:
	      - /etc/ssh/sshd_config
	
	
	  - name: Disable password authentication for root
	    lineinfile:
	      path: /etc/ssh/sshd_config
	      state: present
	      regexp: '^#?PermitRootLogin'
	      line: 'PermitRootLogin prohibit-password'
	
	  - name: "reload sshd"
	    ansible.builtin.systemd_service:
	      name: sshd
	      state: reloaded
	  
	```
5. Setup ssh ansible playbook
	```sh
	- hosts: all
	  become: true
	  tasks:

	  - name: Change port SSH
	    lineinfile:
	      path: /etc/ssh/sshd_config
	      state: present
	      regexp: 'Port 22'
	      line: 'Port 1234'

	  - name: UFW - Allow SSH connections
	    community.general.ufw:
	      rule: allow
	      port: '1234'

	  - name: UFW - Enable and deny by default
	    community.general.ufw:
	      state: enabled
	      default: deny        
	  
	  - name: "reload sshd"
	    ansible.builtin.systemd_service:
	      name: sshd
	      state: reloaded
	  
	```
6. UFW allow example Appserver
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2017-31-04.png?raw=true)
	
