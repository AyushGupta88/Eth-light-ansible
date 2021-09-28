# Eth-light-ansible
This is the ansible playbook to automatically setup eth light node over docker in remote server.
## The steps for proceeding further are as follows :-
### STEP-1 Install ansible in your system
For installing ansible follow the below official documentation.
[Ansible Installation](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
### STEP-2 Update the inventory file
If the server is an EC2 instance the we have to write the inventory file as follows-

```
[ec21]
<EC2 instance IP> ansible_ssh_user=<SSH username> ansible_connection=ssh  ansible_ssh_private_key_file=<EC2 instance private key path(.pem file)> ansible_python_interpreter=/usr/bin/python3
```
Replace the values of the following according to your instance details
1. EC2 instance IP
2. SSH username
3. EC2 instance private key path(.pem file)
### STEP-3 Running the Playbook
Clone the repository in your local system.
The directory structure after cloning will be as shown below
```
.
├── docker-compose.yml
├── docker.yml
├── eth.yml
└── master.yml

0 directories, 4 files

```
### NOTE:-change the paths in files wherever required.
Now we have the custom dockerfile for bitcoin node, docker-compose, bitcoin configuration, and playbook.
A master.yml file which runs two playbooks 
1. docker.yml
2. eth.yml

The `docker.yml` playbook setup docker and docker-compose into the remote server if not present.

The `eth.yml` setup eth-light node over docker in remote server.

To run the playbooks
```
#ansible-playbook master.yml
```
To run the playbook with verbose 
```
#ansible-playbook master.yml -vv
```
#Increase or decrease the number of v for depth of verbosity you want.

## Output of the playbook should look something as shown below
![output1](https://github.com/AyushGupta88/Eth-light-ansible/blob/f59444ca7e0877e388b05caa19dffac2de2c69bb/Screenshot%20from%202021-09-28%2014-58-10.png)


Also ssh to the server and check for the node to be deployed over docker.

```
ubuntu@ip-172-31-84-130:~/opt/ethlight$ sudo docker-compose ps
     Name                    Command               State                                                                       Ports                                                                     
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
ethlight_geth_1   geth --rinkeby --rpc --rpc ...   Up      0.0.0.0:30304->30303/tcp,:::30304->30303/tcp, 0.0.0.0:30304->30303/udp,:::30304->30303/udp, 0.0.0.0:7545->8545/tcp,:::7545->8545/tcp, 8546/tcp

```
To check the syncing of the node 
get inside the docker container
```
#docker-compose exec geth sh
```
Attach to the node using following command it will open up a javascript console.
```
#geth attach http://127.0.0.1:8545
```
For checking syncing run the following command in the console
```
#eth.syncing
```
If it is syncing will show the below output.

Similalrly using tab button you can see all the available options for command line with eth.
Thank You
