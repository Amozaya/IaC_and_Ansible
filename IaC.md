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
