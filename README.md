## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![alt text](https://github.com/Satuppe1/ELK-Stack-Deployment/blob/master/Diagrams/Red-Team%20Network%20Diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the elk_playbook.yml file may be used to install only certain pieces of it, such as Filebeat.

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

## Description of the Topology  

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing plays an important role in security.  It ensures that the application will be highly available by distributing incoming network traffic across multiple backend servers to defend against distributed denial-of-service attacks (DDoS).  This ensures no single servers bear too much demand and possibly breaks down. 

The jump box can be accessed via port 22 and serves as an SSH gateway to the network, isolating access to a private network.

Integrating an ELK (Elasticsearch, Logstash, and Kibana) server allows users to easily monitor the vulnerable VMs for changes to the servers and system logs.

Filebeat helps to generate and organize log files to send the output to logstash and Elasticsearch.

Metricbeat collects metrics and statistics on the system, such as uptime, CPU usage and services running on the server and sends the output to Logstash and Elasticsearch.

The configuration details of each machine may be found below.

| Name     | Function   | IP Address | Operation System |
|----------|------------|:----------:|------------------|
| Jump Box | Gateway    | 10.0.0.4   | Linux            |
| Web-1    | Web Server | 10.0.0.10  | Linux            |
| Web-2    | Web Server | 10.0.0.11  | Linux            |
| Web-3    | Web Server | 10.0.0.12  | Linux            |
| Red-Elk  | ELK Stack  | 10.1.0.4   | Linux            |

## Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the local workstation public IP address: 96.253.65.11.

Machines within the network can only be accessed by the Jump Box (10.0.0.4) via SSH (port 22) using Ansible public keys.

A summary of the access policies in place can be found in the table below.

| Name          | Publicly Accessible | Allowed IP Address |
|---------------|---------------------|:------------------:|
| Jump Box      | No                  | 96.253.65.11       |
| Web-1         | No                  | 10.0.0.4           |
| Web-2         | No                  | 10.0.0.4           |
| Web-3         | No                  | 10.0.0.4           |
| Red-Elk       | No                  | 96.253.65.11       |
| Load Balancer | Yes                 | Any                |

## Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because Ansible is agentless. It does not require agents to be installed on remote systems in order to manage or configure the systems.  Making it efficient and consistent with each deployment.  

The playbook implements the following tasks:
- Config Elk VM with Docker
- Increase Virtual Memory
- Install Docker
- Install pip3
- Install Docker Python module
- Download and launch a docker elk container
- Enables docker service

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.



![alt text](https://github.com/Satuppe1/ELK-Stack-Deployment/blob/master/Images/Docker%20ps%20Screenshot.png)

## Target Machines & Beats

This ELK server is configured to monitor the following machines:
- Web-1 (10.0.0.10)
- Web-2 (10.0.0.11)
- Web-3 (10.0.0.12)

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:

- Filebeat monitors and collects data about the file system.  It enables analysts to monitor files for suspicious activity.

- Metricbeat records metrics and services running on the server.

## Using the Playbook

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
  1. Copy the configuration file to Ansible container.

  2. Update the Ansible `hosts` file to make Ansible run the playbook on a specific machine.

```
 /etc/ansible/hosts
 [webservers]
 10.0.0.4 ansible_python_interpreter=/usr/bin/python3
 10.0.0.5 ansible_python_interpreter=/usr/bin/python3
 10.0.0.6 ansible_python_interpreter=/usr/bin/python3

 [elk]
 10.1.0.4 ansible_python_interpreter=/usr/bin/python3
```
   4. Create the `elk_playbook.yml` file.  Copy the playbook to the `/etc/ansible/roles` directory.  This is the directory that will contain the ansible playbooks.

   5. Run the playbook, and navigate to `http://52.160.91.182:5601/app/kibana` to check that the installation worked as expected.

   6. Steps for Filebeat Installation:
   - Install Filebeat.
   - Create the Filebeat configuration file.
   - Create the filebeat installation playbook.
   - Verify the installation and playbook.

   7.  Repeat for Metricbeat Installation.
 
How do you specify which machine to install the ELK server on versus which to install Filebeat on?
In order to use Ansible to configure the ELK server you need to edit the inventory file `nano /etc/ansible/hosts`.  Add a group call `[elk]` and specify the IP address of the VM you created.  Once you created the `[elk]` group,  create a playbook and configure it.  The `hosts` option in the header specifies which machines to run a set of tasks against, which is the `elk` group.  This allows you to run certain playbooks on some machines, but not on others.

```---
- name: Config elk VM with Docker
  hosts: elk
  remote_user: sysadmin
  become: true
  tasks:
  - name: Install Packages
  # Etc.
```  

Commands to download the playbook, update the files, etc.

Use the following command to connect to your jump box.
```
ssh sysadmin@<jump box external IP>
```
Use the following commands to connect to the Ansible container in the Jump Box.

sudo docker container list -a
sudo docker start <container_name>
sudo docker attach <container_name>

Use the following command to run your playbook.
	
ansible-playbook  /etc/ansible/roles/elk_playbook.yml

Use the following command to edit or update your playbook.

nano /etc/ansible/roles/elk_playbook.yml
 



	

