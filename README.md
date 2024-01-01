Prerequisites
-------------

1.  **EC2 Instances:**

    -   Create three EC2 instances on Amazon AWS: one as the Ansible master and two as hosts.

    -   ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703921146388/11331802-9b67-4e6f-858a-2eaee6180efc.png?auto=compress,format&format=webp)

2.  **SSH to Master Instance:**

    -   Connect to the Ansible master instance using SSH.

```
ssh -i "your-key.pem" ubuntu@your-master-ip

```

[](https://arjunmenon.hashnode.dev/day-55-56-configuration-management-with-ansible#heading-installing-ansible "Permalink")
--------------------------------------------------------------------------------------------------------------------------

Installing Ansible
------------------

1.  **Install Ansible:**

    -   Add the Ansible repository and install Ansible.

```
sudo apt-add-repository ppa:ansible/ansible
sudo apt update
sudo apt install ansible

```

1.  **Verify Installation:**

    -   Confirm that Ansible is installed successfully.

```
ansible --version

```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703921167963/0affe71c-1b14-4301-8941-0982aad62425.png?auto=compress,format&format=webp)

[](https://arjunmenon.hashnode.dev/day-55-56-configuration-management-with-ansible#heading-setting-up-ansible-configuration "Permalink")
----------------------------------------------------------------------------------------------------------------------------------------

Setting Up Ansible Configuration
--------------------------------

1.  **Transfer PEM File:**

    -   Copy the PEM file from your local machine to the master instance.

```
scp -i "your-key.pem" your-key.pem ubuntu@your-master-ip:/home/ubuntu/keys

```

1.  **Edit Ansible Hosts File:**

    -   Open the Ansible hosts file for editing.

```
sudo vim /etc/ansible/hosts

```

1.  **Configure Hosts:**

    -   Add the following lines to the hosts file to define the hosts in a group named 'servers.'

```
[servers]
host_1 ansible_host=your-host-1-ip
host_2 ansible_host=your-host-2-ip

[all:vars]
ansible_user=ubuntu
ansible_ssh_private_key_file=/home/ubuntu/keys/your-key.pem
ansible_python_interpreter=/usr/bin/python3

```

1.  **Change PEM File Permissions:**

    -   Restrict permissions on the PEM file.

```
chmod 400 your-key.pem

```

1.  **Ping Hosts:**

    -   Verify connectivity by pinging the hosts in the 'servers' group.

```
ansible -m ping servers

```

-   The ping should be successful.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703921198489/4f2da9cd-711f-4da4-a0c0-93651964e60c.png?auto=compress,format&format=webp)

[](https://arjunmenon.hashnode.dev/day-55-56-configuration-management-with-ansible#heading-ansible-ad-hoc-commands "Permalink")
-------------------------------------------------------------------------------------------------------------------------------

Ansible Playbook Overview
-------------------------

```
---
- name: Install Nginx and deploy a custom webpage
  hosts: your_ubuntu_servers  # Replace with your target server(s) or inventory group
  become: true  # Run tasks with elevated privileges

  tasks:
    - name: Update package cache
      apt:
        update_cache: yes

    - name: Install Nginx
      package:
        name: nginx
        state: present

    - name: Start Nginx service
      service:
        name: nginx
        state: started
        enabled: yes

    - name: Copy custom webpage files
      copy:
        src: /path/to/your_webpage_files/
        dest: /var/www/html/

```

[](https://arjunmenon.hashnode.dev/day-59-ansible-project#heading-explanation "Permalink")
------------------------------------------------------------------------------------------

Explanation
-----------

1.  **Update package cache:**

    -   Ansible task using the `apt` module to update the package cache on the target servers.
2.  **Install Nginx:**

    -   Ansible task utilizing the `package` module to install Nginx on the target servers.
3.  **Start Nginx service:**

    -   Ansible task with the `service` module to start the Nginx service on the target servers and enable it to start on boot.
4.  **Copy custom webpage files:**

    -   Ansible task using the `copy` module to copy the custom webpage files from your specified source directory to the destination directory on the target servers.

Running the Playbook: Execute the following command to run the playbook:

```
ansible-playbook -i /etc/ansible/hosts deploy_webpage.yml

```

[](https://arjunmenon.hashnode.dev/day-59-ansible-project#heading-verification "Permalink")
-------------------------------------------------------------------------------------------

Verification
------------

After the playbook execution is complete, visit the public IP of any host server in your web browser to verify if the webpage has been successfully deployed.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704094679067/2166eb8a-33cc-4ffa-821b-9bc1715712b0.png?auto=compress,format&format=webp)

[](https://arjunmenon.hashnode.dev/day-59-ansible-project#heading-conclusion "Permalink")
-----------------------------------------------------------------------------------------
