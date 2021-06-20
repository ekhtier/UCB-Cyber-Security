# UC Berkeley Cyber Security Bootcamp Project 1
## Authored by Sheikh Ahmed
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![alt text](https://github.com/ekhtier/UCB-Cyber-Security/blob/main/Images/Project-1%20Network%20Diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the configuration file may be used to install only certain pieces of it, such as Filebeat.

  - [filebeat-config](Ansible/filebeat-config.yml)
  - [filebeat-playbook](Ansible/filebeat-playbook.yml)
  - [metricbeat-config](Ansible/metricbeat-config.yml)
  - [metricbeat-playbook](Ansible/metricbeat-playbook.yml)
  - [install-elk](Ansible/install-elk.yml)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the Damn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting exposing to the network.
- Load Balancers provide Availability aspect of security of CIA triad
- Jump box is like Gateway interface, it's used for system administration and protect internal network.

- The advantage of Jump box is that we can monitor and control other VMs from a single point. If we need to change the security or routing policy, we can make the changes in jump box, rather than changing in all machines. It sits in front of other machine. So we can control access to other machine by allowing specific IP address and forwarding to those machines.  

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the filesystem and system resources.
- Filebeat generates log information about the filesystem including which files are changed and send it to Logstash and Elasticsearch for indexing. 
- Metricbeat collects metrics and statistical data from operating system and services and send them to Logstash or Elasticsearch. 

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.1.0.4   | Linux Ubuntu 18.4            |
| Web-1    | DVWA     | 10.1.0.5   | Linux Ubuntu 18.4|
| Web-1    | DVWA     | 10.1.0.6   | Linux Ubuntu 18.4|
| Elk      | Network monitoring |10.2.0.4| Linux Ubuntu 18.4    |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Admin's public IP address

Machines within the network can only be accessed by SSH.
- Only Jump box provisioner was allowed to access ELK VM and its IP is 10.2.0.4

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | Admin's Public IP    |
| Web-1    | No                  | 10.1.0.4             |
| Web-2    | No                  | 10.1.0.4             |
| Elk Server| Yes                  | Admin's Public IP|

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- The main advantage of automating configuration with Ansible is that we don't need to set up each individual application separately.

The playbook implements the following tasks:

- Install docker.io
- Install python3-pip
- Install docker module
- Increase virtual memory
- Download and launch docker ELK container


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![alt text](https://github.com/ekhtier/UCB-Cyber-Security/blob/main/Images/docker_ps_output.PNG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines: 
| Name     | IP Addresses |
|----------|---------------------|
| Web-1    | 10.1.0.5             |
| Web-2    | 10.1.0.6             |

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat is a lightweight shipper that monitors log files, reads log for new contents and forwards new log data to Elasticsearch or Logstash. Filebeat logs information about the file system.
- Metric beat collects metric and statistics from the operating system and services. It generates report on CPU usage, memeory usage etc.  
 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-playbook.yml file to /etc/ansible/roles/.
- Update the filebeat-config.yml file to include the ELK server IP 10.2.0.4 to Elasticsearch and Kibana setup
- Run the playbook, and navigate to http://52.149.230.108:5601/app/kibana to check that the installation worked as expected.

We need to run the following commands to configure ELK server and monitor log files in Kibana portal. 

- ssh -i ~/.ssh/id_rsa azadmin@[jumpbox public IP]
- sudo docker container start nervous_wilbur
- sudo docker container attach nervous_wilbur
- cd /etc/ansible/
- ansible-playbook Install_elk.yml (this will configure Elk-Server) 
- cd /etc/ansible/roles/
- ansible-playbook filebeat-playbook.yml (installs Filebeat)
- Bring up the Kibana Web Portal by navigating http://52.149.230.108:5601/app/kibana
