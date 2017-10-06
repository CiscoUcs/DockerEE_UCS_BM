# DockerEE_UCS_BM
Docker Enterprise Edition On UCS BareMetal
# Ansible Playbook for Docker-EE on UCS BareMetal Servers
Ansible code for deploying Docker Enterprise Edition
This repo contains Ansible play-book code for deploying Docker Enterprise Edition v17.06 on UCS bare metal nodes. Following pre-requisites/important notes, are needed before the play-book can be played on -

  1. UCS Manager Service Profile configuration is out of scope of this playbook. It is expected that Day0/1 tasks for setting up UCS servers, storage configuration, network configuration have already been taken up. 
  2. Bare Metal OS with RHEL7.3 has been taken care of beforehand.
  3. All the operating system have atleast 2 disks, one for OS boot and the other one for Docker Enterprise usage.
  4. Playbook expects to run from a build server having Ansible version 2.3.2 installed and have connectivity to all the target hosts w/ password less SSH access. To setup password less ssh access pls refer - https://access.redhat.com/solutions/9194
  5. It expects some of the environement variable to be setup as global-vars, details are captured in respective sections.
  6. This playbook is written for environment which works behind proxy, if not needed certain sub-tasks should be skipped.
  7. Same is the case with certain environemental files like /etc/hosts, Cisco VIC enic driver RPM etc to be present in files sections of certain roles.
  8. Play-book does all POST OS installation tasks including storage configurations and Docker EE installs. 
  9. Play-book is written with UCS-Manager version 3.2(1d) and corresponding Cisco VIC - enic driver for RHEL7.3.
  
  
. Ansible play-book tree structure -

          .
          ├── DEE-Nodes
          ├── DEE-Nodes.yml
          ├── group_vars
          │   └── all
          └── roles
              ├── common
              │   ├── files
              │   │   ├── bash_profile
              │   │   ├── environment
              │   │   ├── hosts
              │   │   ├── kmod-enic-2.3.0.39-rhel7u3.el7.x86_64.rpm
              │   │   ├── ntp.conf
              │   │   └── rhsm.conf
              │   └── tasks
              │       └── main.yml
              ├── docker
              │   ├── files
              │   │   ├── daemon.json
              │   │   └── http-proxy.conf
              │   └── tasks
              │       └── main.yml
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

          31 directories, 24 files

. Swarm/UCP cluster formation and DTR installations need to be done separately after running this playbook as of now. In near future we will add these 2 tasks as well into existing playbook. 

. To run the playbook, one need to download entire directory structure on a build node and run following command, after editing DEE-Nodes file with cluster nodes details, putting values inside group_vars/all file -

  `ansible-playbook -verbose -i /etc/ansible/DEE-Nodes DEE-Nodes.yml -u root` 
