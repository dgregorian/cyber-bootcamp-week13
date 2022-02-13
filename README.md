## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

- [Network Diagram](images\Azure Diagram - BootCamp.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

- [install-elk.yml](\ansible-playbook\install-elk.yml)
- [Other Files](\ansible-playbook\)

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly [available], in addition to restricting [inbound traffic] to the network.
- What aspect of security do load balancers protect? Protect against DDoS attacks that may otherwise saturate a single device.
- What is the advantage of a jump box?_ Allows you to block all entry via load balancer and create a secure means of connection through your own "back door".  Safer than allowing direct communciation to the web servers via the load balancer.  

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the servers and system logs.
- What does Filebeat watch for?_ Filebeat parses server logs and sends them to a centralized server to simplify administration.
- What does Metricbeat record?_ Metricbeat parses server performance metrics (process utilization, memory, disk, etc) and sends the data to a centralized server to allow monitoring health of your web servers.

The configuration details of each machine may be found below.

| Name          | Function      | Private Addrs | Pub Addrs      | Operating System |
|---------------|---------------|---------------|----------------|------------------|
| Load Balancer | Load Balancer | <N/A>         | 168.62.167.192 | Azure Service    |
| Jump Box      | Gateway       | 10.0.0.4      | 20.115.19.42   | Linux            |
| Web-1         | Web Server    | 10.0.0.5      | <N/A>          | Linux            |
| Web-2         | Web Server    | 10.0.0.6      | <N/A>          | Linux            |
| Web-3         | Web Server    | 10.0.0.7      | <N/A>          | Linux            |
| ELK-SERVER    | Monitor       | 10.1.0.4      | <N/A>          | Linux            |
| teneble-nessus| Net. Scanner  | 10.1.0.5      | <N/A>          | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the [Jump Box] machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 47.28.120.76

Machines within the network can only be accessed by Jump Box.
Which machine did you allow to access your ELK VM? What was its IP address?_ The jump box at 10.0.0.4.  I also allowed external communication from my home IP, but I would not have if this were a production environment.

A summary of the access policies in place can be found in the table below.

| Name           | Publicly Accessible | Allowed IP Addresses |
|----------------|---------------------|----------------------|
| Jump Box       | Yes (SSH)           | 47.28.120.76         |
| Load Balancer  | Yes (HTTP)          | 47.28.120.76         |
| Elk Server     | Yes (HTTP)          | 47.28.120.76         |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
 What is the main advantage of automating configuration with Ansible?_ You can quickly deploy and configure large numbers of servers from a central point.  If there were hundreds of containers this would significantly simplify the management.

The playbook implements the following tasks:
- Increases available memory.
- Installs a docker container with ELK pre-configured.
- Publishes the ports so the host forwards traffic to container.
- Enable the service so it starts on boot.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

- [Elk Docker Container](\images\elk-docker-ps-output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines: 10.0.0.5, 10.0.0.6, 10.0.0.7

We have installed the following Beats on these machines:  Filebeats, Metricbeats.

These Beats allow us to collect the following information from each machine:  Filebeats pulls log files from the webservers, metricbeats pulls the CPU, Memory utlization.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the [config + playbook] file to [the ansible directory].
- Update the [hosts] file to include [your target servers]
- Run the playbook, and navigate to [http://20.119.219.4:5601/] to check that the installation worked as expected.

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

1) cd /etc/ansible/
2) nano hosts - Update the hosts file with your target servers.
3) nano ansible.cfg - Update the ansible.cfg file to fine-tune settings (e.g. setting the default user to attempt connection as)
4) ssh <user>@<webserver-IP> - Ensure connection via key is established with target servers by remotely executing a "ping" command or SSHing into the boxes.
5) curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat >> /etc/ansible/filebeat-config.yml
6) nano filebeat-config.yml - replace the appropriate instances with your Kibana server's IP address.
7) curl https://uclax.bootcampcontent.com/UCLA-Coding-Boot-Camp/ucla-la-virt-cyber-pt-10-2021-u-c/-/blob/master/1-Lesson-Plans/13-Elk-Stack-Project/Activities/Stu_Day_2/Solved/config_files/filebeat-playbook.yml >> /etc/ansible/filebeat-playbook.yml
8) ansible-playbook filebeat-playbook.yml