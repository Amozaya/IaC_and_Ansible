# Infrastructure as Code (IaC)

## What is IaC

IaC is a process of managing and provisioning of computer infrastructure through the code, instead of manually. 

With IaC you can crete configuration files with infrastructure specification, so it's easier to edit and distribute it. Also, by using Configuration files, you ensure that every time  you provision the same environment. 

IaC uses version control as well, so any changes you do to the configuration files should be under different versions to ensure you can return back to the previous version if needed.

Automating infrastructure provisioning with IaC means that developers don't need to manually provision the servers each time they develop and deploy the app.


### Benefits of IaC

* Cost reduction
* Increase in speed of deployments
* Reduce errors
* Improve infrastructure consistency
* Eliminate configuration drift

### Use cases of IaC

* In software development life cycle. When the environments are uniform the chance of the bugs is much lower, as well as it reduces the time required for deployment and configuration of all the environments. 


### Configuration management

Configuration management is the process of configuring provisioned infrastructure resources. For example, configuring a server with required application.

The primary goal is to configure a server. You can use tools like Ansible in order to install and configure an application automatically (e.g., Nginx).


### What is Ansible

![Ansible diagram](resources/AnsibleDiagram.JPG)

Ansible is an open source automation tool that automates provisioning, configuration management, application deployment and many more other processes. Ansible can be used to automate software installation, daily tasks, provisioning, system patching etc.

### Ansible playbook
A playbook is a file written in YAML that provides instructions for what needs to be done in order to bring a managed node into the desired state.

### Ansible benefits
* Simple to learn
* Easily understandable Python language
* No dependency on Agents
* Playbook is written in YAML



## Creating Ansible controller and Web/Db nodes

1. Create a project in VS Code
2. Use bash terminal to run the `vagrant init` command to initialise the project folder, which will create a Vagrant file
3. Inside the Vagrant file replace the config code with the following code:
```
# -*- mode: ruby -*-
 # vi: set ft=ruby :
 
 # All Vagrant configuration is done below. The "2" in Vagrant.configure
 # configures the configuration version (we support older styles for
 # backwards compatibility). Please don't change it unless you know what
 
 # MULTI SERVER/VMs environment 
 #
 Vagrant.configure("2") do |config|
  # creating are Ansible controller
    config.vm.define "controller" do |controller|
      
     controller.vm.box = "bento/ubuntu-18.04"
     
     controller.vm.hostname = 'controller'
     
     controller.vm.network :private_network, ip: "192.168.33.12"
     
     # config.hostsupdater.aliases = ["development.controller"] 
     
    end 
  # creating first VM called web  
    config.vm.define "web" do |web|
      
      web.vm.box = "bento/ubuntu-18.04"
     # downloading ubuntu 18.04 image
  
      web.vm.hostname = 'web'
      # assigning host name to the VM
      
      web.vm.network :private_network, ip: "192.168.33.10"
      #   assigning private IP
      
      #config.hostsupdater.aliases = ["development.web"]
      # creating a link called development.web so we can access web page with this link instread of an IP   
          
    end
    
  # creating second VM called db
    config.vm.define "db" do |db|
      
      db.vm.box = "bento/ubuntu-18.04"
      
      db.vm.hostname = 'db'
      
      db.vm.network :private_network, ip: "192.168.33.11"
      
      #config.hostsupdater.aliases = ["development.db"]     
    end
  
  
  end
```

This code will create 3 separate VMs: controller, web, and db

4. Use `vagrant up` to start the VMs. Ensure that you run VirtualBox as Administrator

5. Open GitBash terminal as Administrator and `cd` into the folder where you Vagrantfile is
6. You can use `vagrant status` in order to check what VMs are running
7. Use `vagrant ssh <VM_name>` in order to connect to the VM. For example `vagrant ssh controller`
8. Once you connected run `sudo apt update -y` and `sudo apt upgrade -y`
9. Repeat previous two steps for each VM. You can open a new GitBash terminal to make it easier
10. Once all the machines are updated, go back to your `controller` VM
11. Use `python --version` to check what version of Python is installed. You need at least 2.7.x up to 3.6.x
12. Run `sudo apt install software-properties-common`
13. Run `sudo apt-add-repository ppa:ansible/ansible`
14. Run `sudo apt update -y`
15. Run `sudo apt instal ansible -y`
16. Use `ansible --version` to ensure Ansible is installed and it is correct version
17. You can use `ssh vagrant@<VM_IP>` to connect to the specific VM through your controller. It will ask you to add the password for your connection. *!When typing the password it wont be visible in the CLI for security reason!*

## Use controller and agent nodes with Ansible
1. Use `Vagrant up` to start VMs and then connect to `controller` VM vis GitBash terminal using `vagrant ssh controller`
2. `cd /etc/ansible/` - navigate to default Ansible directory
3. `ls` - check in `ansible.cnf` and `hosts` exists
4. `sudo apt install tree` - install tree package
5. `tree` - use the command to see the list of files instead of `ls`
6. `sudo ansible all -m ping` - check connections with agent nodes:
  * `-m` - module
  * `all` - looks for all agent nodes
  * `ping` - ping the agent nodes to check connection

7. `ssh vagrant@192.168.33.10` - connect to `web` VM
8. Upgrade and update `web` VM
```
sudo apt update -y`
sudo apt upgrade -y
```
9. `exit` - return back to controller
10. `ssh vagrant@192.168.33.11` - connect to `db` VM
11. Update and upgrade the `db` VM
```
sudo apt update -y
sudo apt upgrade -y
```
12. `exit` - return back to controller
13. `pwd` - use to ensure you are still in `etc/ansible/` directory

14. `sudo nano hosts` - open hosts file

15. Add the following lines of code: 
```
[web]
192.168.33.10
```
16. `sudo ansible web -m ping` - will give an error about public key
17. `sudo nano hosts` - to go back to hosts file
18. `192.168.33.10 ansible_connection=ssh ansible_ssh_user=vagrant ansible_ssh_pass=vagrant` - we verify the connection: type ssh, user vagrant, and password vagrant"
19. `sudo ansible web -m ping` - now this time connection should work

20. `sudo nano hosts`
21. Add following lines to connect to `db` VM
```
[db]
192.168.33.11 ansible_connection=ssh ansible_ssh_user=vagrant ansible_ssh_pass=vagrant`
```
  If connection is not verified do the following steps:
  * `sudo nano ansible.cfg`
  * add `host_key_checking = false` under `[defaults]`
22. `sudo ansible all -m ping` - now both VMs should be connected and responding

### Ansible ad hoc commands

* `sudo ansible all -a "date"` - date and timezone for each machine
* `sudo ansible all -a "uname -a"` - information about each machine
* `sudo ansible all -a "free"` - shows free memory on each machine


* `sudo nano test.txt` - create test txt file
* add `# testing data transfer from controller to web-vm using adhoc command`
* use`sudo ansible web -m copy -a "src=/etc/ansible/test.txt dest=/home/vagrant"` to copy file over to `web` VM:
  * `web` - agent node where we want to copy to
  * `-m copy` - module copy, that will create a copy of the file
  * `-a "src=/etc/ansible/test.txt dest=/home/vagrant"` - attribute that specifies a source, where file located on the controller, and the destination folder, where file will be copied on the `web` machine


  ## Creating YAML playbook

  1. `sudo nano install-nginx-playbook.yaml` - create new YAML playbook

  2. Add following script:

  ```

  # creating a playbook to install nginx web server

  # YAML file starts with ---
  ---
  # where would you like to install nginx
  - hosts: web

  # would you like to see logs (optional)
    gather_facts: yes

  # do we need admin access "sudo"
    become: true

  # add the instructions - commands
  tasks:
    - name: Install nginx in web-server

      apt: pkg=nginx state=present

  # ensure status is active

  ```

3. `sudo ansible-playbook install-nginx-playbook.yml` - run yaml playbook

4. `sudo ansible web -a "systemctl status nginx"` - check status of nginx on the `web` VM





