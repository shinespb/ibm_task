VM with nagios
============


Just clone: 

```bash
git clone https://bitbucket.org/shinespb/ansible_task.git
```

## What is prerequisites for vagrant?

* vagrant (vagrantup.com), >= 1.5.3
* virtualbox
* ansible (ansible.com) >= 1.9

## what prerequisites for EC2 run?

* ansible >= 1.9
* boto(package for python) (pip install boto)

## What does it do?

Creates local virtual machine with vagrant or in EC2 cloud.

## How I could run it with vagrant?

Just go to directory, where you've cloned this code and run
```bash
vagrant up
```
Vagrant creates virtual machine and setup nagios on it with custom modules, mediawiki and apache.

## How I could create VM in cloud?

Go inside cloned directory and run:
```
export ANSIBLE_HOST_KEY_CHECKING=no; ansible-playbook -i hosts -u=ubuntu ec2_install.yaml
```

## Where I could see result?
If you're runnig VM with vagrant, just open browser and go to
```
http://127.0.0.1:3000
```

If you're running VM with EC2, in ansible run you could 
se an IP address for connection.

## What services is on this VMs? Where I could find them?

There is a monitoring service:
```
/nagios3
```
and mediawiki with a little bit of instructions.
```
/mediawiki
```

## I've installed VM with vagrant and made some changes in playbooks. How I could make it update?

It's simple, just run
```bash
vagrant provision
```

## What if I want to update EC2 VM?

Sorry, this is not implemented now :( 

## What could I customize?

You could change everything, but if you want to do it well, edit group_vars/all. In this file you could redefine any other variables

## Oh, I've found a bug. What I could do?

Ok, don't worry. Just send me a e-mail with description of problem and I'll fix it.
shinespb@gmail.com

## What about patterns plugin? It everytime shows PATTERNS CRITICAL: no directories for this time 

Patterns is in /events_storage/phishing_constant_patterns. Scripts take current time and and count files in directories for N time bask. If it shows "PATTERNS CRITICAL: no directories for this time" you need to change local time to check old directories(in sample) or just add some actual patterns

