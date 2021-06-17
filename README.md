## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

! [ELK Network Diagram](https://github.com/Mason-cmyk/ELK/blob/5b1d97f9c1d82f03149ef65bebba54cdb88e69e1/Diagrams/ELK%20Network%20Diagram.jpg)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

  - filebeat-playbook.yml

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
- Load balancers protect servers from DoS attacks. A jumpbox protects your VMs from being exposed to the public.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the log files and system files.
- Filebeat keeps watch on your log files


The configuration details of each machine may be found below.


| Jump Box | Gateway   | 10.0.0.4 | Linux Ubuntu |
|----------|-----------|----------|--------------|
| DVWA VM1 | VM        | 10.0.0.5 | Linux Ubuntu |
| DVWA VM2 | VM        | 10.0.0.6 | Linux Ubuntu |
| ELK VM   | ELK Stack | 10.1.0.4 | Linux Ubuntu |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Public IP

Machines within the network can only be accessed by Port 22.
- Jumpbox/10.0.0.4

A summary of the access policies in place can be found in the table below.

| Name      | Publicly Accessible | Allowed IP Addresses |
|-----------|---------------------|----------------------|
| JumpBox   | No                  | 10.0.0.4             |
| DVWA VMs  | No                  | 10.0.0.5/10.0.0.6    |
| ELK Stack | No                  | 10.1.0.4             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Ansible ensures our scripts are run across all machines.

The playbook implements the following tasks:
- Add memory to ELK VM
- Installs docker.io
- Installs python-pip
- Installs docker python module
- Downloads and launches ELK stack

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

!https://github.com/Mason-cmyk/ELK/blob/ca47c853e5693edb53d8d427301222e14ca68863/Images/docker_ps_output.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5
- 10.0.0.6

We have installed the following Beats on these machines:
- filebeat-7.6.1-darwin-x86_64

These Beats allow us to collect the following information from each machine:
- Filebeat collects and sends log files from specified servers to Kibana.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-config.yml file to ELK VM.
- Update the hosts file to include 10.0.0.5 and 10.0.0.6
- Run the playbook, and navigate to Kibana to check that the installation worked as expected.


- filebeat-playbook.yml
- ansible-playbook
- ipaddress@kibana.com

---
  - name: filebeat installer
    hosts: webservers
    become: true
    tasks:
    
    - name: download filebeat
      shell: curl -L -O  https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb
            
    - name: install filebeat
      shell: dpkg -i filebeat-7.6.1-amd64.deb 

    - name: Copy filebeat configuration
      copy:
       src: /etc/ansible/files/filebeat-configuration.yml
       dest: /etc/filebeat/filebeat.yml
       owner: root
       group: root
       mode: '0600'
       backup: yes

    - name: restart filebeat
      shell: filebeat modules enable system
   
    - name: filebeat setup
      shell: filebeat setup
  
    - name: filebeat -e
      shell: filebeat -e &
      ```
