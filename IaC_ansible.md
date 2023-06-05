
IAC helps us to codify any mannual process. 


## Ansible 

Ansible is a configuration manager 

It is a extremely powerful, simple, and agentless automation tool

**Powerful** - Ansible can manage and facilitate 2 to thousands of servers. You can have different tasks being conducted on different servers all at the same time

**Simple** - Ansible only requires a few lines of code 

**Agentless** - It is agentless, meaning that only the controller needs to have ansible installed. This is beneficial ass if you have lots of servers, e.g., 200, all of these do not need to have ansible installed, helpting to save lots of time. 


## Setting up nodes 

1. Download the vagrantfile provided into the same folder as your README.md for IaC

2. Run Vagrant up (if prompted to create a vagrant file, copy contents of downloaded vagrant file into the new one you create)

3. Run 'vagrant up'

Make sure you have Virtual Box open. As 'vagrant up' is happening, you should start seeing the 3 vms (controller, web, and db) come up and eventually say 'running'.

4. To check the status of the vms, use the command:

```
vagrant status
```

5. SSH into each vm (one after the other), and run:

```
sudo apt-get update -y
sudo apt-get upgrade -y
```

## Setting up the Ansible Controller

1. First we need to ssh into the controller:
```
ssh vagrant@ 192.168.33.11
```

2. we also need to install common packages, add the Ansible repo, and install ansible:

```

sudo apt install software-properties-common

sudo apt-add-repository ppa:ansible/ansible

sudo apt-get update -y

sudo apt install ansible -y
```

To check if ansible 2.9.27 is installed do 'sudo ansible --version'


**note** you only need to install ansible on the controller. 

3. You can then configure it so that it can use ssh to communicate to the other 2 VMs (web and db)

```
ssh vagarant@<ip-adress>
```

4. When you log into one of these vms, it will ask you to add it to the known hosts (so say yes to this and input the password 'vagrant')

5. Next we need to go to the right place:

```
cd /etc/ansible
```

6. We then need to add the key for the web and db vm:

```
sudo nano hosts
```

7. In the terminal that appears, add the two lines:

```
[web]

192.168.33.10 ansible_connection=ssh ansible_ssh_user=vagrant ansible_ssh_pass=vagrant

[db]

192.168.33.11 ansible_connection=ssh ansible_ssh_user=vagrant ansible_ssh_pass=vagrant
```

![Alt text](keys.PNG)

8. Save and exit this.

9. Then test the connections (to get pings):

```
sudo ansible all -m ping
```
![Alt text](Pings.PNG)


## Blockers

If this did not work, try the following:

```
sudo nano ansible.config
```

From here, find the default section, and remove the '#' from the 'host_key_checking=false' line.

You can also go to the SSH conection section and add 'host_key_checking=false' 

You can then run the 'sudo ansible all -m ping' command again to test if it was successful. 


