## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.!

[Azure diagram wih ELK](https://user-images.githubusercontent.com/91027892/134097440-e426c749-5fa2-4e02-a8b5-fc4e13dd397c.PNG)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Playbook file may be used to install only certain pieces of it, such as Filebeat.

[Ansible Configuration.txt](https://github.com/Ollego/Azure-Virtual-Network/files/7199507/Ansible.Configuration.txt)
[Ansible Hosts.txt](https://github.com/Ollego/Azure-Virtual-Network/files/7199509/Ansible.Hosts.txt)
[Ansible Playbook.txt](https://github.com/Ollego/Azure-Virtual-Network/files/7199511/Ansible.Playbook.txt)
[ELK Installation.txt](https://github.com/Ollego/Azure-Virtual-Network/files/7199513/ELK.Installation.txt)
[Filebeat Configuration.txt](https://github.com/Ollego/Azure-Virtual-Network/files/7199514/Filebeat.Configuration.txt)
[Filebeat Playbook.txt](https://github.com/Ollego/Azure-Virtual-Network/files/7199515/Filebeat.Playbook.txt)
[Metribeat Configuration.txt](https://github.com/Ollego/Azure-Virtual-Network/files/7199516/Metribeat.Configuration.txt)
[Metricbeat Playbook.txt](https://github.com/Ollego/Azure-Virtual-Network/files/7199517/Metricbeat.Playbook.txt)


This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting traffic to the network.
-  Load Balancer helps distribute traffic evently across the servers and mitigates DoS attacks. 
-  Jump Box is exposed to the public internet and sits in front of other VMs that are not exposed to the public internet (serves as a gateway). It controls access to the other VMs by allowing connections from specific IP addresses and forwarding to those VMs.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
-  Filebeat monitors specified log files or locations, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.
-  Metricbeat collects metrics from the operating system and from services running on the server.

The configuration details of each machine may be found below.

| Name     | Function                   | IP Address            | Operating System |
|----------|----------------------------|-----------------------|------------------|
| Jump Box | Gateway                    | 10.0.0.4  13.91.1.16  | Linux            |
| Web-1    | Web Server/ DVWA container | 10.0.0.5              | Linux            |
| Web=2    | Web Server/ DVWA Container | 10.0.0.6              | Linux            |
| ELK      | Elk Server/ Monitoring     | 10.1.0.4  13.90.45.91 | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box and ELK Server machines can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 
76.29.91.192

Machines within the network can only be accessed by Jump Box.
Jump Box has access to the Elk Server via SSH port 22. Jump Box internal IP address is 10.0.0.4

A summary of the access policies in place can be found in the table below.

| Name       | Publicly Accessible | Allowed IP Addresses                                 |
|------------|---------------------|------------------------------------------------------|
| Jump Box   |         Yes         |               76.29.91.192 SSH port 22               |
| Web-1      |          No         |                 10.0.0.4 SSH port 22                 |
| Web=2      |          No         |                 10.0.0.4 SSH port 22                 |
| ELK Server |         Yes         | 10.1.0.4 SSH port 22     76.29.91.192 TCP port 5601  |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- The automation process improves the speed and drastically reduces the potential of human error and simplifies the process of configuring many machines identically all at once.

The playbook implements the following tasks:
- Sets the vm.max_map_count to 262144
- Installs the following apt packages:
   -docker.io
   -python3-pip
- Installs the following pip packages:
   -docker
- Downloads the Docker container called sebp/elk:761. sebp
- Configures the container to start with the following port mappings:
    5601:5601
    9200:9200
    5044:5044
- Starts the container
-  Enables the docker service on boot, so that if you restart your ELK VM, the docker service start up automatically.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![dockerps](https://user-images.githubusercontent.com/91027892/134108205-76019ef5-0b4c-4c79-be1f-2ecc88f291b5.PNG)


### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1 10.0.0.5
- Web-2 10.0.0.6

We have installed the following Beats on these machines:
- Filebeats
- Metricbeats

These Beats allow us to collect the following information from each machine:
Filebeats helps generate and organize log files to send to Logstash and Elasticsearch. It logs information about the file system, including which files have changed and when. 
Metricbeats helps generate logs for system-level CPU usage, memory, file system, disk IO, and network IO statistics, as well as top-like statistics for every process running on the machine.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the YAML file to /etc/ansible.
- Update the /etc/ansible/hosts file to include host group, private IP address and the folliwng 'ansible_python_interpreter=/usr/bin/python3'
- Run the playbook, and navigate to Kibana to check that the installation worked as expected.

YAML file is for playbooks. In this project I used  install-elk.yml, filebeat-playbook.yml and metricbeat-playbook.yml. Thease files are copied to /etc/ansible directory.

To make Ansible run the playbook on a specific machine, update the hosts file. This file uses square brackets to specify the groups with IP addresses identified for each group. After that the hosts can be identified in the playbook file.  

To check if the ELK server is running, navigate to http://[your.VM.IP]:5601/app/kibana


Specific commands the user will need to run to download the playbook
- sudo apt install docker.io  to install Docker container
- sudo docker pull cyberxsecurity/ansible to pull the ansible container
- sudo su to switch to root user
- docker run -ti cyberxsecurity/ansible:latest bash  to start the container
- exit to quit
- nano /etc/ansible/hosts  to access the hosts file
  Uncomment the [webservers] header line.
  Add the internal IP address under the [webservers] header.
  Add the python line: ansible_python_interpreter=/usr/bin/python3 besides each IP.
-  Ctrl + x to exit file
-  curl -L -O https://ansible.com/  > ansible.cfg to download Ansible Configuration file
-  nano /etc/ansible/ansible.cfg  to change the Ansible configuration file to use your administrator account for SSH connections.
    Uncomment the remote_user line and replace root with your admin username using this format: remote_user = <user-name-for-web-VMs>
-  touch /etc/ansible/install-elk.yml  this command creates a new playbook
-  curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/filebeat-config.yml to download filebeat configuration file
- curl https://gist.githubusercontent.com/slape/58541585cc1886d2e26cd8be557ce04c/raw/0ce2c7e744c54513616966affb5e9d96f5e12f73/metricbeat > /etc/ansible/files/metricbeat-config.yml to download Metricbeat file
  -  ansible-playbook /etc/ansible/pentest.yml  to run pentest playbook (replace pentest.yml with a desired playbook file to run that playbook)
