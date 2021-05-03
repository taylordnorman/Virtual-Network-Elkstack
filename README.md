## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Virtual Network](https://github.com/taylordnorman/Virtual-Network-Elkstack/blob/main/Ansible/Images/Virtual_Network_Diagram_Project.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the my-playbook.yml file may be used to install only certain pieces of it, such as Filebeat.


This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly redundant, in addition to restricting unauthorized access to the network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file system and system metrics such as uptime.

The configuration details of each machine may be found below.

| Name                 | Function                  | Ip Address | Operating System    |
|----------------------|---------------------------|------------|---------------------|
| Jump-Box-Provisioner | Gateway                   | 10.0.0.4   | Linux/Ubuntu Server |
| Elk-VM               | Elk Stack                 | 10.1.0.4   | Linux/Ubuntu Server |
| Web-1                | Webserver containing DVWA | 10.0.0.5   | Linux/Ubuntu Server |
| Web-2                | Webserver containing DVWA | 10.0.0.6   | Linux/Ubuntu Server |
| Web-3                | Webserver containing DVWA | 10.0.0.9   | Linux/Ubuntu Server |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box-Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
76.115.149.31

Machines within the network can only be accessed by the Jump-Box-Provisioner from 10.0.0.4 IP address.
The Elk-vm can only be accessed by the Jump-Box-Provisioner at 40.78.6.158 and my local network at 76.115.149.31.

A summary of the access policies in place can be found in the table below.

Name                 | Publicly Accessible | Allowed IP Addresses                       |
|----------------------|---------------------|------------------------------------------|
| Jump-Box-Provisioner | Yes                 | 76.115.149.31 10.0.0.5 10.0.0.6 10.0.0.9 |
| Elk-VM               | Yes                 | 76.115.149.31 40.78.6.158                |
| Web-1                | No                  | 10.0.0.4                                 |
| Web-2                | No                  | 10.0.0.4                                 |
| Web-3                | No                  | 10.0.0.4                                 |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because using Ansible allows you to create one playbook file that you can run on all machines to ensure each instance is identical. This allows automation when adding services to a system, ensures they are all identical, and saves on time by instantly adding to all systems at once.

The playbook implements the following tasks:

- Install docker.io utilizes the apt command ensuring to get docker onto the container
- Install docker module utilizes pip which is used to manage python library dependencies in this instance it checks that the state is present on docker, if not then it installs docker.
- Install python3.pip is using apt get to install python3.pip from the repository if it is not already installed.
- Download and launch a docker elk container uses docker container to manage the lifecycle of docker containers. Using image:sebp/elk:761 is used as a repository path to create the container if the image is not found and on the device.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![Docker ps -a](https://github.com/taylordnorman/Virtual-Network-Elkstack/blob/main/Ansible/Images/Dockerps.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5
- 10.0.0.6
- 10.0.0.9

We have installed the following Beats on these machines:
Filebeat
Metricbeat

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the my-playbook.yml file to /etc/Ansible.
- Update the /etc/ansible/hosts file to include, seperating [webserver] and [elk] allows you to run the ansible command on target groups only.

[webservers]
10.0.0.5 ansible_python_interpreter=/usr/bin/python3
10.0.0.6 ansible_python_interpreter=/usr/bin/python3
10.0.0.9 ansible_python_interpreter=/usr/bin/python3

[elk]
10.1.0.4 ansible_python_interpreter=/usr/bin/python3

- Run the playbook, and navigate to http://(your.VM.IP):5601/app/kibana. to check that the installation worked as expected.
