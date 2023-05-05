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