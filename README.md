## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.


![Image of Network Diagram](https://github.com/patmckernan/Project-1/blob/main/images/Network-Diagram.png)


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the cloud environment file may be used to install only certain pieces of it, such as Filebeat.

  
 [Filebeat Playbook](https://docs.google.com/document/d/1Vd87GFKld5CJpdCCVJa3g8_pBcIMst9NS-dW2OGLu4U/edit?usp=sharing)
 
 
  
This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly redundant, in addition to restricting a single point of failure to the network.
Load balancing prevents having a single point of failure on a network. 

The advantage of a Jump-Box VM is to create a single point of entry that can be secured with firewalls generated from azure network security groups. Load balancing protects against DoS attacks, and provide redundancy if one server were to become offline. 


Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the actual machines and system logs.

- Filebeat enables the organization of log files to be generated and sent to Logstash, Kibana, and Elasticsearch. 

- Metricbeat periodically collects meterics from the system services and operating system on a machine. For example Metricbeat will collect services such as Apache, MongoDB, MySQL, and PostgreSQL.

The configuration details of each machine may be found below. 

| Name        	| Function         	| IP Address 	| Operating System 	|
|-------------	|------------------	|------------	|------------------	|
| Jump-Box VM 	| Gateway          	| 10.0.0.4   	| Linux            	|
| Web-1       	| DVWA Containers  	| 10.0.0.5   	| Linux            	|
| Web-2       	| DVWA Containers  	| 10.0.0.6   	| Linux            	|
| Web-3       	| DVWA Containers  	| 10.0.0.7   	| Linux            	|
| Elk-1       	| Configuration VM 	| 10.1.0.4   	| Linux            	|

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box VM machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

- My host machine IP address: 172.14.134.91

Machines within the network can only be accessed by accessing the DVWA container throuh Jump-Box VM. 

- The only machine that can access is Elk-1 server is Jump-Box VM through DVWA container through a peering connection with origin IP 10.0.0.4.

A summary of the access policies in place can be found in the table below.


| Name        	| Publicly Accessible 	| Allowed IP Addresses    	|
|-------------	|---------------------	|-------------------------	|
| Jump-Box VM 	| Yes                 	| 172.14.134.91, 10.0.0.4 	|
| Web-1       	| No                  	| 10.0.0.4                	|
| Web-2       	| No                  	| 10.0.0.4                	|
| Web-3       	| No                  	| 10.0.0.4                	|
| Elk-1       	| No                  	| 10.0.0.4                	|

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

The benefit of automating the configuration of machines with Ansible is that it reduces the amount of error when configuring multiple machines at one time manually. Additionally, it simplifies the deployment of software, firewall systems, and available ports for client machines. 

The playbook implements the following tasks:

- Install docker
- Download image
- Configure ansible container
- Create ansible playbook
- Run ansible playbook to launch container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.


![Image of Elk docker status](https://github.com/patmckernan/Project-1/blob/main/images/Elk%20docker%20status.png)



### Target Machines & Beats
This ELK server is configured to monitor the following machines:

- Web-1 VM 10.0.0.5
- Web-2 VM 10.0.0.6
- Web-3 VM 10.0.0.7

We have installed the following Beats on these machines:

We have installed both Filebeat, and Metricbeat on these machines. 

These Beats allow us to collect the following information from each machine:

Filebeat monitors the log files of choosen systems. It then forwards this data to Logstash, or Elasticsearch. This data is able to be viewed via Kibana. Kibana is able to display information in the below screenshot. 

![Image of Filebeat status](https://github.com/patmckernan/Project-1/blob/main/images/Filebeat.png)
  
Metribeat is is a lightweight shipper that sends data on the status of system services. When metricbeat does not receive these metrics it will send an error event from the host system. 

![Image of Metricbeat status](https://github.com/patmckernan/Project-1/blob/main/images/Metricbeat.png)

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/filebeat-config.yml

SSH into the control node and follow the steps below:
- Copy the filebeat-playbook.yml file to /etc/ansible/roles.
- Update the filebeat-config.yml file to include ELK-1 private IP 10.1.0.4 under hosts in Elasticsearch output, and Kibana host.
- Run ansible-playbook filebeat-playbook.yml, and navigate to Kibana (http://104.214.89.46:5601/app/kibana) via Elk-1 public IP 104.214.89.46 to check that the installation worked as expected.

In order to specify which machine to run a playbook on you would have to edit /etc/ansible/hosts with the proper group, and internal IP addresses of the selected machines.

http://104.214.89.46:5601/app/kibana#/home will be the URL to navigate to in order to ensure that the ELK server is running. 

The following commands are instructions on how to download, install, update the files required to run an ansible playbook to configure multiple VM's autonomously. 

- ssh into Jump-Box VM: ssh azdmin@52.158.230.227
- Install, setup, launch Docker.io using the following commands:
    sudo apt install docker.io
    sudo docker pull cyberxsecurity/ansible
    sudo docker run -ti cyberxsecurity/ansible:latest bash        #Launches the ansible container
    sudo docker container list -a                                 #Finds the ansible continer name
    sudo docker start jolly_hertz                                 #My container was named jolly_hertz
    sudo docker attach jolly_hertz
    
  ![Image of Docker](https://github.com/patmckernan/Project-1/blob/main/images/Docker.png)
    
 - Edit the Config and Hosts File
    nano /etc/ansible/hosts
    root@fb2552ff5c72:~# nano /etc/ansible/hosts
    
  Uncomment the [webservers] header
  Under the webservers header add the internal IP addresses of the VM's.
     [webservers]                                                                                                                                                       10.0.0.5 ansible_python_interpreter=/usr/bin/python3                                                                                                               10.0.0.6 ansible_python_interpreter=/usr/bin/python3                                                                                                               10.0.0.7 ansible_python_interpreter=/usr/bin/python3
     
  nano /etc/ansible/ansible.cfg
      root@fb2552ff5c72:/# nano /etc/ansible/ansible.cfg
      
  scroll to line 106 and change remote user to azdmin (my VM username.) Save and exit.
      remote_user = azdmin

- Download and Configure the container
    Created a playbook called ansible_config.yml
  
  root@fb2552ff5c72:/etc/ansible# cat ansible_config.yml                                                                                                                                                                          ---                                                                                                                                                                                                                             - name: Config Web VM with Docker                                                                                                                                                                                                 hosts: webservers                                                                                                                                                                                                               become: true                                                                                                                                                                                                                    tasks:                                                                                                                                                                                                                            - name: docker.io                                                                                                                                                                                                                 apt:                                                                                                                                                                                                                              update_cache: yes                                                                                                                                                                                                               name: docker.io                                                                                                                                                                                                                 state: present                                                                                                                                                                                                                                                                                                                                                                                                                                              - name: Install pip3                                                                                                                                                                                                              apt:                                                                                                                                                                                                                              name: python3-pip                                                                                                                                                                                                               state: present                                                                                                                                                                                                                                                                                                                                                                                                                                              - name: Install Docker python module                                                                                                                                                                                              pip:                                                                                                                                                                                                                              name: docker                                                                                                                                                                                                                    state: present                                                                                                                                                                                                                                                                                                                                                                                                                                              - name: download and launch a docker web container                                                                                                                                                                                docker_container:                                                                                                                                                                                                                 name: dvwa                                                                                                                                                                                                                      image: cyberxsecurity/dvwa                                                                                                                                                                                                      state: started                                                                                                                                                                                                                  restart_policy: always                                                                                                                                                                                                          published_ports: 80:80                                                                                                                                                                                                                                                                                                                                                                                                                                      - name: Enable docker service                                                                                                                                                                                                     systemd:                                                                                                                                                                                                                          name: docker                                                                                                                                                                                                                    enabled: yes                            
    
- Run the playbook once the editing is complete.
    
  ![Image of Ansible playbook](https://github.com/patmckernan/Project-1/blob/main/images/Ansible%20playbook.png)  
 
     
     
 
     
