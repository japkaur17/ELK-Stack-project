# ELK-Stack-project
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below: 

![Cloud Architecture](https://github.com/japkaur17/ELK-Stack-project/blob/main/images/ELK%20NW.PNG) 

[NOTE: the network diagram has differing IP addresses from the ones Azure assigned me by default]

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook (.yml) file(s) may be used to install only certain pieces of it, such as Filebeat.


This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting inbound access to the network. The load balancer ensures that resources required to process incoming traffic will be shared by both vulnerable web servers. Access controls will ensure that only authorized users will be able to connect. Load Balancing ensures availability to the web-servers which is the availability aspect of security in regards to the CIA Triad.



- What aspect of security do load balancers protect? What is the advantage of a jump box?_

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.
- What does Filebeat watch for?
- Filebeat monitors the log files or locations specified, collects log events, and forwards them for indexing
- What does Metricbeat record?
- Metricbeat records system and application metrics. It allows for viewing cpu, memory, disk and network metrics, in addition to also allowing monitoring for apache, docker, and applications installed. It also records statisctical data from the operating system and from services running on the server (Metricbeat: Lightweight Shipper for Metrics)

The main advantage of using a jump box is having one origination point for administrative tasks. This ultimately sets the JumpBox as a Secure Admin Workstation, in order to conduct administrative tasks required to access the JumpBox before accessing the other servers.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.1   | Linux            |
| DVWA-1   | WebServer| 10.0.0.5   | Linux            |
| DVWA-2   | WebServer| 10.0.0.6   | Linux            |
| ELK      | Monitor  | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBox Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: My Public IP



Machines within the network can only be accessed by each other. The DVWA machines (Web-1 and Web-2) can send traffic to the ELK-Server


A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box |    Yes              |    My Public IP      |
| DVWA-1   |    No               |    10.1.0.1-254      |
| DVWA-2   |    No               |    10.1.0.1-254      |
| ELK      |    No               |    10.1.0.1-254      |

In addition to the above, Azure has provisioned a load balancer in front of all machines except for the JumpBox. The load balancer's targets are organized into the following availability zones: 

Availability Zone 1: Web-1 + Web-2  Availability Zone 2: ELK 

### Elk Configuration

The ELK VM exposes an Elastic Stack Instance. Docker is used to download and manage an ELK container.

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because this allows for easy replication and portability. 
Also, by automating the installation process we could deploy multiple servers easily and quickly without having to physically touch each server.

The Ansible playbook implements the following tasks: 
Installing Docker.io, Installing pip3, Installing the Docker Python Module, Setting the system memory necessary for this machine to work properly, and Downloading and launching a Docker ELK container

The Ansible playbook can be found in the YAML file elk.yml in this repo, and is also included here for reference.
![pentestyml](https://github.com/japkaur17/ELK-Stack-project/blob/main/images/pentest%20yml.PNG)


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.
![Docker](https://github.com/japkaur17/ELK-Stack-project/blob/main/images/server.PNG) 

### Target Machines & Beats
This ELK server is configured to monitor the following machines: Web-1 and Web-2 (Both running DVWA)
- List the IP addresses of the machines you are monitoring_

We have installed the following Beats on these machines:
- Specify which Beats you successfully installed_
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat watches for log files, locations and collect log events.
- Metricbeat records metrics and statistical data from the operating system and from servers runnings on the server

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-config.yml and metricbeat-config.yml file to /etc/ansible/files.
- Update the configuuration files to include the Private IP of the ELK-Server to the ElasticSearch and Kibana Sections of the Configuration File
- Run the playbook, and navigate to ELK-Server-PublicIP:5601/app/kibana to check that the installation worked as expected.


- The playbook files are: 
 - elk-playbook.yml - used to install ELK server
  - filebeat-playbook.yml - Used to install and configure Filebeat on ELK Server and DVWA servers
  - metricbeat-playbook.yml - Used to install and Configure Metricbeat on ELK Server and DVWA servers 

- Where do you copy it :  /etc/ansible 
- Which file do you update to make Ansible run the playbook on a specific machine? :  /etc/ansible/hosts.cfg
- How do I specify which machine to install the ELK server on versus which to install Filebeat on?:  In /etc/ansible/hosts user writes instruction where to install the ELK servers or Filebeat
- Which URL do you navigate to in order to check that the ELK server is running?
http://23.100.44.95:5601/app/kibana#/home/tutorial/systemLogs

_The specific commands the user will need to run to download the playbook, update the files, etc._
- Commands needed to run the Ansible Configuration for the Elk-Server are:
  - ssh RedAdmin@JumpBox (Private IP)
  - sudo docker container list -a  (to locate the ansible container)
  - sudo docker start (e.g.silly_bouman)
  - sudo docker attach (e.g.silly_bouman)
  - cd /etc/ansible/
  - ansible-playbook elk-playbook.yml (Installs and Configure Beats)
  - cd /etc/ansible/
  - ansible-playbook beats-playbook.yml (Installs and Configure Beats)
  - Open a new browser on personal workstation, navigate to (ELK-Server-Public:5601/app/kibana) - This will bring up Kibana Web Portal 
![Kibana](https://github.com/japkaur17/ELK-Stack-project/blob/main/images/kibana.PNG) 
![Kibanalogs](https://github.com/japkaur17/ELK-Stack-project/blob/main/images/logs.PNG)


Congratulations! You have an ELK Stack Server monitoring system logs on two web servers

 

 
