# DockerEE_UCS_BM
Docker Enterprise Edition On UCS BareMetal
# Ansible Playbook for Docker-EE on UCS BareMetal Servers
Ansible code for deploying Docker Enterprise Edition
This repo contains Ansible play-book code for deploying Docker Enterprise Edition v17.06 on UCS bare metal nodes. Following pre-requisites/important notes, are needed before the play-book can be played on -

  1. UCS Manager Service Profile configuration is out of scope of this playbook. It is expected that Day0/1 tasks for setting up UCS servers, storage configuration, network configuration have already been taken care of. 
  2. Bare Metal OS with RHEL7.3 has been taken care of before hand.
  3. All the operating system have atleast 2 disks, one for OS boot and the other one for Docker Enterprise usage.
  4. Playbook expects to run from a build server having Ansible version 2.3.2 installed and have connectivity to all the target hosts w/ password less SSH access. To setup password less ssh access pls refer - https://access.redhat.com/solutions/9194. In case user doesn't want to run the YUM role, they can run the playbook from anyof the cluster nodes. No need of a separate build server in that case. They have to make sure password-less SSH access from that node to itself and rest of the nodes have been taken care of. Also user has to remove the entry for build server from group_vars/all.
  5. It expects some of the environement variable to be setup as global-vars, details are captured in respective sections.
  6. This playbook is written for environment which works behind proxy, if proxy is not needed certain sub-tasks should be skipped.
  7. Same is the case with certain environemental files like /etc/hosts, Cisco VIC enic driver RPM etc to be present in files sections of certain roles.
  8. Play-book does all POST OS installation tasks including storage configurations, firewall and Docker EE installs. 
  9. Play-book is written with UCS-Manager version 3.2(1d) and corresponding Cisco VIC - enic driver for RHEL7.3.
  10. All required configuration files for DEE has been taken care of using ansible templates. 
  
  
. Ansible play-book tree structure -

	.
	├── DEE-C-Nodes
	├── DEE-C-Nodes.yml
	├── DEE-Nodes
	├── DEE-Nodes.yml
	├── group_vars
	│   └── all
	└── roles
    	├── common
    	│   ├── files
    	│   │   ├── kmod-enic-2.3.0.39-rhel7u3.el7.x86_64.rpm
    	│   │   └── kmod-enic-2.3.0.44-rhel7u3.el7.x86_64.rpm
    	│   ├── tasks
    	│   │   └── main.yml
    	│   └── templates
    	│       ├── bash_profile.j2
    	│       ├── environment.j2
    	│       ├── hosts.j2
    	│       ├── ntp.conf.j2
    	│       └── rhsm.conf.j2
    	├── docker
    	│   ├── files
    	│   │   └── daemon.json
    	│   ├── tasks
    	│   │   └── main.yml
    	│   └── templates
    	│       └── http-proxy.conf.j2
    	├── firewall
    	│   └── tasks
    	│       └── main.yml
    	├── ntp
    	│   └── tasks
    	│       └── main.yml
    	├── storage
    	│   └── tasks
    	│       └── main.yml
    	├── UCPdtr
    	│   └── tasks
    	│       └── main.yml
    	├── UCPdtr-r1
    	│   └── tasks
    	│       └── main.yml
    	├── UCPdtr-r2
    	│   └── tasks
    	│       └── main.yml
    	├── UCPreplica
    	│   ├── files
    	│   └── tasks
    	│       └── main.yml
    	├── UCPswarm
    	│   ├── files
    	│   │   └── docker_subscription.lic
    	│   └── tasks
    	│       └── main.yml
    	├── UCPworker
    	│   ├── files
    	│   └── tasks
    	│       └── main.yml
    	└── yum
        └── tasks
            └── main.yml

33 directories, 27 files

. To run the playbook, one need to download entire directory structure on a build node and run following command, after editing DEE-Nodes file with cluster nodes details, putting values inside group_vars/all file -

  `ansible-playbook -verbose -i /etc/ansible/DEE-Nodes DEE-Nodes.yml -u root` 
