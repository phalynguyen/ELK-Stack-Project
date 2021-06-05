# ELK-Stack-Project
The files in this repository were used to configure the network depicted below.



![](https://github.com/phalynguyen/ELK-Stack-Project/blob/main/Images/ELK%20Stack%20Diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the .yml file may be used to install only certain pieces of it, such as Filebeat.

  - These playbooks were ran in oder to deploy the ELK-Server: [elk.yml](https://github.com/phalynguyen/ELK-Stack-Project/blob/main/Ansible/elk.yml), [filebeat-playbook.yml](https://github.com/phalynguyen/ELK-Stack-Project/blob/main/Ansible/filebeat-playbook.yml)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
- Load balancing protects against DDoS attacks, this shifts traffic from the attack from the primary corporate server to a public cloud service. This allocates the availabilities amoungst servers to ensure continuation of services if one server goes down. 
- The advantage of having a jump box is that it provides a secure access to other servers through the SSH protocol.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.
- Filebeat monitors locations and log files, collects log event data, and sends that data to Elasticsearch or Logstash.
- Metricbeat documents the statistics and metrics that it collects, and sends them to an output. (i.e., Elasticsearch or Logstash)

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| Web-1    | Server   | 10.0.0.7   | Linux            |
| Web-2    | Server   | 10.0.0.6   | Linux            |
| ELK- VM  | Server   | 10.1.04    | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 104.215.97.119

Machines within the network can only be accessed by The Jump Box Provisioner VM.
- Private IP: 10.0.0.4

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | No                  | Private IP           |
| Web-1    | No                  | 10.0.0.4             |
| Web-2    | No                  | 10.0.0.4             |
| ELK-VM   | No                  | 10.0.0.4             |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Using the Ansible playbook allows for multiple tasks to be automated and will configure multiple machines by running a single command through the playbookks.

The playbook implements the following tasks:
- Curl the Filebeat Configuration file into the Ansible container
- Edit configuration to include all servers involoved in implementation
- Create Playbook considering Linux Filebeat installation
- Run Playbook
- Create inbound sercurity rule on the server to allow Port 5601

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![](https://github.com/phalynguyen/ELK-Stack-Project/blob/main/Images/docker%20ps%20ELK.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.7
- 10.0.0.6

We have installed the following Beats on these machines:
- Filebeat 

These Beats allow us to collect the following information from each machine:
- Filebeat monitors and logs data that you allow and transfers them to Elasticsearch

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the `filebeat-config.yml` file to `etc/ansible/files` directory in the Ansible container.
- Update the  file to include the host IP for the ELK server (10.0.0.4) to port 9200 which is the API over HTTP that communicates with Elasticsearch  
- Run the playbook, and navigate to the ELK-VM by running SSH to the Private IP of the ELK Server
- The Playbook is the `filebeat-playbook.yml` that is copied to the `etc/anisbile/files` 
- Update `etc/ansible/filebeat-config.yml` file in order to have Ansible run on a specific machine. 
- Navigate to http://[13.88.96.243]:5601/app/kibana to ensure installation worked as expected

### Commands to Intially Run Create ELK Server
- Run `ssh RedAdmin@104.215.97.119`
- Run `sudo docker ps` 
- Run `sudo docker container list -a` to see the name of your container that you will start
- Run `sudo docker container start [Name of container]`
- Run `sudo docker container attach [Name of container]`
- Get the SSH Key from the Ansible Container to copy into Jump Box `cat ~/.ssh/id_rsa.pub`
- Create VM on Azure with this SSH Public Key
- Add new VM to `hosts` file using the python3-pip service `<10.1.0.4> ansible_python_interpreter=/usr/bin/python`
- Create Anible Playbook

