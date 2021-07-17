# elk-stack
PipeDrive Infra. Architect - Assignment

***************************
***************************
**   Deploy ELK Stack    **
***************************
***************************


Description
===========

This project contains a script to automate the deployment of an ELK Stack with 3-node ElasticSearch cluster. Also, Zabbix agent is deployed on all nodes to monitor the OS.

Automation tool: Ansible

Tested and applied using the following versions:
================================================
OS: Linux CentOS 7.x
ELK stack version (ES, Kibana, Logstash):  7.5
Zabbix agent version: 5.4

Architecture: (VMs running on VMWare Workstation)
=============
- 3 ElasticSearch indexer nodes forming a single cluster
- 1 Kibana node
- 1 LogStash node
- 1 Zabbix server
- 1 Ansible server (not mandatory)

Project content files:
======================
- hosts.ini 
- group_vars/all.yaml
- elk-stack-zabbix-v1.7.yaml
- my.conf  -------> LogStash custom config file

What do you need to customize first?
===================================
- hosts.ini file: edit the file according to your preferences
- all.yaml: define the variables according to your preferences
- elk-stack-zabbix-v1.7.yaml: edit host IP address variable "{{ ansible_facts.ens33.ipv4.address }}", replace the NIC device name "ens33" with the NIC device name of your operating system
- my.conf: in the "output" section, enter the IPs of the Elasticsearch indexer servers


How to use the script:
======================
- hosts.ini ----------> this file contains a list of the inventory, and it must be located under the same path with the ansible playbook file.
You should enter the hosts you have in your environment in that file.

- all.yaml -----------> this file contains all defined global variables used in the Ansible playbook file.
This file must be located at "ANSIBLE_PLAYBOOK_FILE_PATH/group_vars/all.yaml

This var file defines the following parameters:
	- ElasticSearch IP/hostnames.
	- Elasticsearch desired cluster name.
	- Zabbix server IP addres.
	
- elk-stack-zabbix-v1.7.yaml ------- > This is the Ansible playbook script. This playbook will automate the entire deployment of the ELK Stack.
This playbook file contains five different plays, and here's a quick description of each:
	Play1: Preparing all nodes -->> this play will prepare all nodes in our deployment with the essentials, for example, disabling SELINUX, FIREWALLD, and SWAP, as well as installing some applications, like java, netstat, and Zabbix agent.
	Play2: Deploy ElasticSearch cluster ----> This play will prepare and initialize ElasticSearch cluster on member nodes. In this project, all ES nodes are configured as masters, data, and ingest nodes.
	Play3: Preparing KIBANA INSTANCE
	Play4: Deploy LogStash node
	Play5: Monitor the status of ElasticSearch cluster nodes ---> this play queries the status of ElasticSearch nodes
	
Important note: the playbook file contains a variable that grabs the IP address of the nodes. Please make sure to modify that variable to specify the NIC device name in your operating system. The variable is: {{ ansible_facts.ens33.ipv4.address }}

- my.conf: this file defines INPUT/FILTER/OUTPUT parameters and the listening port of LogStash server. You can rename this file according to your preferences.

Running the script:
===================
	ansible-playbook elk-stack-zabbix-v1.7.yaml -i hosts.ini
	
Thank you,
	
	


