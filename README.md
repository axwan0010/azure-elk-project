## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Network Diagram](Images/Network_Diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the config and yml file may be used to install only certain pieces of it, such as Filebeat.

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting traffic to the network.

Aspects of security that load balancers protect: Availability, Web Traffic, Web Security

Advantage of a jump box:  Automation, Security, Network Segmentation, Access Control


Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
- Filebeat monitors the log files or locations that you specify.
- Metricbeat records the metrics and statistics from the operation system and from services running on the server.

The configuration details of each machine may be found below.

| Name                 | Function     | IP Address               | Operating System |
|----------------------|--------------|--------------------------|------------------|
| Jump-Box-Provisioner | Gateway      | 10.0.0.4 /13.73.197.17   | Linux            |
| Web-1                |Web Server    | 10.0.0.5                 | Linux            |
| Web-2                |Web Server    | 10.0.0.6                 | Linux            |
| ELK-SERVER           |ELK Server    | 10.1.0.4 /168.63.243.204 | Linux            |
| Load Balancer        |Load Balancer | 13.75.199.142            | Linux            | 
### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the ELK-SERVER  machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 

- Work Station public IP through port 5601

Machines within the network can only be accessed by Jump-Box-Provisioner and Workstation have allowed access to ELK-SERVER.

- Jump-Box-Provisioner IP : 10.0.0.4 through SSH port 20
- Workstation Public IP through TCP port 5601

A summary of the access policies in place can be found in the table below.

| Name        | Publicly Accessible  | Allowed IP Addresses                  |
|-------------|----------------------|---------------------------------------|
| Jump Box    |      No              | Workstation Public IP on SSH  p22     |
|   Web-1     |      No              | 10.0.0.4 on SSH  22                   |
|   Web-2     |      No              | 10.0.0.4 on SSH  22                   |
| ELK-SERVER  |      No              | Workstation Public IP using TCP p5601 |
|Load balancer|      No              | Workstation Public IP on  HTTP p80    |  

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

- Ansible allows automating tasks, which saves a lot of time. IT admins can quickly deploy mutiple configuration throug the ansible playbook and Ansible will automatically 
  check the system state and update the system base on the playbook content.
  
The playbook implements the following tasks:
- Install docker.io
- Install python3-pip
- Install Docker python module
- Set the vm.max_map_count to 262144
- Download and launch a docker elk container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

!

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1, 10.0.0.5
- Web-2, 10.0.0.6


We have installed the following Beats on these machines:
- Metricbeat
- Filebeat


These Beats allow us to collect the following information from each machine:
 - Filebeat:  Mointors log events
 - Metricbeat:  Records metrics and system statistics


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the ansible.cfg file to /etc/ansible
- add the machine, its IP, and ansible_python_interpreter=/usr/bin/python3 to the hosts in the ansible.cfg as shown below:
 # /etc/ansible/hosts
 [webservers]
10.0.0.4 ansible_python_interpreter=/usr/bin/python3
10.0.0.5 ansible_python_interpreter=/usr/bin/python3
10.0.0.6 ansible_python_interpreter=/usr/bin/python3

 [elk]
 10.1.0.4 ansible_python_interpreter=/usr/bin/python3

- Copy the install-elk.yml and filebeat-playbook.yml file to /etc/ansible.
- Update the install-elk.yml and filebeat-playbook.yml file to include the machine you want use the playbooks on by changing the hosts name on the 3rd line. 
- Run the playbook, and navigate to  http://[your.VM.IP]:5601/app/kibana to check that the installation worked as expected.

### Commands to Use the Playbook
- nano ansible.cfg
- add the machine, its IP, and ansible_python_interpreter=/usr/bin/python3 to the hosts
- Ctrl + x to exit file
- in the folder that install-elk.yml is in, run: cp install-elk.yml /etc/ansible
- nano install-elk.yml /etc/ansible
- ---
  - name: installing elk
  hosts: [your_machine]
- Ctrl + x to exit file
- ansible-playbook install-elk.yml



