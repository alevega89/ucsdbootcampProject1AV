## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below 

![This is my Diagram](https://github.com/alevega89/ucsdbootcampProject1AV/blob/main/Diagrams/Network_Map.png)


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YML file may be used to install only certain pieces of it, such as Filebeat.

  Ansible/Install metric beat.txt
  Ansible/filebeat install.txt
  linux/elk install.txt
  
This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly monitored, in addition to restricting traffic to the network.


Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the services system and services running in the system.
	Filbeats watch for log directories or specific log files.
	Metricbeat helps you monitor your servers by collecting metrics from the system and services running on the server.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function  | IP Address | Operating System |
|----------|-----------|------------|------------------|
| Jump Box | Gateway   | 10.0.0.5   | Linux            |
| Web 1    | Server    | 10.0.0.8   | Linux            |
| Web 2    | Server    | 10.0.0.9   | Linux            |
| ELK      | Log Server| 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
Personal IP Address

Machines within the network can only be accessed by Jump-Box.
Elk Machine can have access from personal IP address through port 5601.

A summary of the access policies in place can be found in the table below.

| Name          | Publicly Accessible | Allowed IP Addresses |
|---------------|---------------------|----------------------|
| Jump Box      | Yes/No              | 10.0.0.1 10.0.0.2    |
| Load Balancer | Yes                 | OPEN                 |
| Web 1         | No                  | 10.0.0.8             |
| Web 2         | No                  | 10.0.0.9             |
| Elk Server    | Yes                 | 10.1.0.4             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because system installation and updates can be streamlined, and processes are easier to repeat.

The playbook implements the following tasks:
  
# Use apt module
    - name: Install docker.io
      apt:
        update_cache: yes
        name: docker.io
        state: present

      # Use apt module
    - name: Install pip3
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present

      # Use pip module
    - name: Install Docker python module
      pip:
        name: docker
        state: present

      # Use sysctl module
    - name: Use more memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
        state: present
        reload: yes

      # Use docker_container module
    - name: download and launch a docker elk container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always
        published_ports:
          - 5601:5601
          - 9200:9200
          - 5044:5044

      # Use systemd module
    - name: Enable service docker on boot
      systemd:
        name: docker
        enabled: yes
		

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

elk server screenshot.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
Web 1 (10.0.0.5)
Web 2 (10.0.0.6)

We have installed the following Beats on these machines:
FileBeat
Metric Beat

These Beats allow us to collect the following information from each machine:
Filebeat is a lightweight shipper for local files. Installed as an agent on your servers, Filebeat monitors the log directories or specific log files, tails the files, and forwards them either to Elasticsearch or Logstash for indexing. An examle of such are the logs produced from the MySQL database supporting our application.
Metricbeat collects metrics and statistics on the system. CPU usage, which can be used to monitor the system health is an example of using metric beats. 
### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the config file to your web vm. 
- Update the /etc/ansible/hosts file to include the IP address of the Elk Server VM and webservers
- Run the playbook, and navigate to http://[Elk_VM_Public_IP]:5601/app/kibana to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_:/etc/ansible/filebeat-config.yml
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?
http://[Elk_VM_Public_IP]:5601/app/kibana


_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
