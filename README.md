# Ansible Automate Project 

## Install Jenkins

First i create an Ec2 instance and name it `Jenkins` and ssh into it

next i update the server

```
sudo apt update
```

![update server](<img/jenkins/1 sudo apt update.png>)

to install `JAVA` i typed the command below to know which java version to install

```
java
```

![java](img/jenkins/java.png)

next i install java for this i installed version 11 using the command below


```
sudo apt install openjdk-11-jre-headless
```

![install java 11](<img/jenkins/2 install java 11.png>)

next i install jenkins on ubutu server.I  used the code located in this website link to do so 

`jenkins.io/doc/book/installing/linux/#debianubuntu`

![jenkins install website](<img/jenkins/3 use jenkins documentation.png>)

using the command on the website i installed jenkins

![install jenkins 1](<img/jenkins/4 install jenkins.png>)


![install jenkins 2](<img/jenkins/5 install jenkins.png>)

next i check the status of jenkins using the command below

```
sudo systemctl status jenkins
```

![jenkins status](<img/jenkins/6 check jenkins status.png>)


if jenkns is active i type the command below into my web browser 

```
<server-public-ip>:8080
```

![visit jenkins server using web browser](<img/jenkins/13 jenkins login page.png>)

to get administrator password i use the command below

```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

![initial jenkins admin password](<img/jenkins/14 locate jenkins password.png>)

next i select install suggested plugins

![install suggested plugins](<img/jenkins/15 next jenkins page.png>)


![plugins getting installed](<img/jenkins/16 plugins getting installed.png>)

afterwhich i create the `First Admin user` inputing `username`, `password`, `fullname`, `email address`

the end result should look like this

![final page of jenkins](<img/jenkins/17 final jenkins page.png>)


<br/>
<br/>
<br/>
<br/>
<br/>
---------------------------------------------------

# Install and configure ansible on ec2 instance

### Step 1 - Install and Configure Ansible on EC2 Instance


I changed the name of my jenkins Ec2 instance to Jenkins-Ansible

![change name to jenkins ansible](<img/1 change name tag to jenkins ansible.png>)


next i Ssh into my Jenkins-Ansible

next i updated my server using the command below

```
sudo apt update
```

![update server](<img/2 sudo apt update.png>)


next i installed ansible


![install ansible](<img/3 install ansible.png>)

next i checked ansible version to confirm ansible is installed

```
ansible --version
```

![ansible version](<img/4 ansible version.png>)


### on jenkins webpage - Configure Jenkins build job to archive your repository content every time you change it

i created a freestyle project

select `new item`

![new item](<img/5 new item.png>)

next enter an `item name` and choose `freestyle project` and select `ok`

![free style project](<img/6 freestyle.png>)


### on Github 

i created a new repository 

![new github repository](<img/7 git hub new account.png>)

click on `settings`

![git settings](<img/8 git settings.png>)

select `webhooks` and `add webhook`

![add webhook](<img/9 git webhook.png>)


on the `payload url` input the following

```
http://<jenkins-server-public-ip-address>:8080/<any-name>/
```

`content type` is `application/json`

`events` is `just the push event`

tick `active`

afterward update workbook

![webhook settings](<img/10 webhook.png>)


### on jenkins webpage

navigate to 

```
http://<jenkins-server-public-ip-address>:8080/job/ansible/configure
```


input the settings shown in the images below

the repository url is the github url repository link created above 

![ansible configure 1](<img/11 ansible configure.png>)

![ansible configure 2](<img/12 ansible configure.png>)

![ansible configure 3](<img/13 ansible configure.png>)

![ansible configure 4](<img/14 ansible configure.png>)

apply and save

on the dashboard i navigate to the `ansible` freestyle project and click on build now and check the build number. if its green the build is correct and free of errors

![error free build now](<img/14 build now and result.png>)


![consile output](<img/16 console output.png>)

![build is successful](<img/17 finished successful.png>)


I tested my setup by making some change in README.md file and made sure that builds starts automatically and Jenkins saves the files (build artifacts) in following folder

![changes made to readme file](<img/18 made some changes to readme file and commit.png>)

![preview change](<img/19 change preview.png>)

![changes reflected](<img/20 new changes automatically recorded.png>)

![changes shown](<img/21 change reflected.png>)

![changes in readme file](<img/22 showed changes made to readme file.png>)

```
sudo ls /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/
```

```
sudo ls /var/lib/jenkins/jobs/ansible/builds/
```
![display build in console](<img/23 show build number and content inside builds.png>)


## Note: Trigger Jenkins project execution only for main (or master) branch.

to see the content of the README.md file on console i used the command below

```
sudo cat /var/lib/jenkins/jobs/Ansible/builds/<build-number>/archive/README.md
```
but first i changed the pwd directory to the archive directory in the image below

![cat build number](<img/24 cat readme file.png>)


#### note: Every time i stop/start my Jenkins-Ansible server - i reconfigured GitHub webhook to the new IP address, in order to avoid repeating this, it makes sense to allocate an Elastic IP to my Jenkins-Ansible server.


## Prepare your development environment using Visual Studio Code

first i inatalled `Remote Development` plugin

![install vscode plugin](<img/25 remote development.png>)

next i connect to my Github repository

![clone repository](<img/26 clone repository.png>)

![select repository locaion](<img/27 select repository location.png>)


### Begin Ansible Development

i checked the github branches in the repository

```
git branch
```

create a new branch

```
git checkout -b <branch-name>
```

create a new directory `playbooks`

```
mkdir playbooks
```

create a new directory `inventory`

```
mkdir inventory
```
![create branch and repositories](<img/28 create new branch and two new directories.png>)


change working directory to playbooks

```
cd playbooks/
```

create common.yml file

```
touch common.yml
```

![craete common.yml file](<img/29 make common yml file in playbook directory.png>)

change working directory to `inventory`

create `dev.yml`, `staging.yml`, `uat.yml`, `prod.yml` file 

![create more yml file](<img/30 playbook creation.png>)


### on windows diretory i added ssh agent

first i changed working directory(pwd) to the location of my private key 

next i typed the command below to obatain `agent pid`

```
eval `ssh-agent -s`
```

```
ssh-add <private-key-name>.pem
```

```
ssh-add bright-usa.pem
```

to connect to the jenkins-ansible instance directly using ssh agent

```
ssh -A ubuntu@<ec2-public-ip-address>
```

![ssh agent](<img/32 connect to my ec2 using perm key.png>)

next is to persist the key on the server

![persist key on server](<img/33 persist key on server.png>)


next i created five(5) new ec2 server with the following description

```
* one nfs server running red-hat os 

* two web-servers running red-hat os

* one database server running red-hat os

* one loadbalancer server running ubuntu os

```

#### i created the instances using the same key pair


![created five ec2 instances](<img/34 created 5 ec2 instance.png>)


i checked if i can connect to the new instances from jenkins server using the command below

```
ssh <os-refrence>@<server-public-ip-address>
```

> os refrence can either be `ubuntu` or `ec2-user`

![check connect 1](<img/35 check if jenkins can connect to any server redhat.png>)


![check connect 2](<img/36 check if jenkins can connect to any server ubuntu i address is private ipaddress.png>)


#### on vscode 

i typed the following command into the dev.yml file 

```
[nfs]
<NFS-Server-Private-IP-Address> ansible_ssh_user='ec2-user'

[webservers]
<Web-Server1-Private-IP-Address> ansible_ssh_user='ec2-user'
<Web-Server2-Private-IP-Address> ansible_ssh_user='ec2-user'

[db]
<Database-Private-IP-Address> ansible_ssh_user='ec2-user' 

[lb]
<Load-Balancer-Private-IP-Address> ansible_ssh_user='ubuntu'
```

![dev.yml file update default](<img/37 update dev yml file.png>)

![set respective ipaddresses](<img/38 updated dev yml file with private ip address.png>)


#### on common Playbook

i typed the command below


```
---
- name: update web, nfs and db servers
  hosts: webservers, nfs, db
  become: yes
  tasks:
    - name: ensure wireshark is at the latest version
      yum:
        name: wireshark
        state: latest
   

- name: update LB server
  hosts: lb
  become: yes
  tasks:
    - name: Update apt repo
      apt: 
        update_cache: yes

    - name: ensure wireshark is at the latest version
      apt:
        name: wireshark
        state: latest
```

```
---
- name: update web, nfs and db servers
  hosts: webservers, nfs, db
  remote_user: ec2-user
  become: yes
  become_user: root
  tasks:
    - name: ensure wireshark is at the latest version
      yum:
        name: wireshark
        state: latest
   
- name: update LB server
  hosts: lb
  remote_user: ubuntu
  become: yes
  become_user: root
  tasks:
    - name: Update apt repo
      apt: 
        update_cache: yes

    - name: ensure wireshark is at the latest version
      apt:
        name: wireshark
        state: latest

```


![edit common yml file](<img/39 edit common yml playbook.png>)


### Updating Github repository

```
git status
```

```
git add .
```

```
git commit -m "<make-any-comment>"
```

```
git push origin <branch-name>
```

![first commit](<img/40 add and commit changes.png>)

i changed my working directory and run the the above git commands 

![git command](<img/41 add and commit changes.png>)

![push origin](<img/42 push request.png>)


afterward i created a pull request

![pull request](<img/43 create pull request other.png>)

![pull request 2](<img/43 create pull request.png>)

![pull request 3](<img/44 create pull request.png>)

![pull request 4](<img/45 merge pull request.png>)

![pull request 5](<img/46 confirm merge.png>)

![pull request merged](<img/47 merged.png>)

![files present](<img/48 files present on main branch.png>)


after a change on my main branch jenkins automatically build artifacts to save the files 

![jenkins build artifact](<img/49 also present on ansible.png>)

contents of files changes can also be seen

![check content of files](<img/50 content also seen on jenkins.png>)

with every changes made a new build number is generated

![build number change](<img/51 build number is ten 10.png>)

to see the changes on the `jenkins-ansible` server i use the command below with respect to the build number to see any changes made and recorded

```
sudo ls /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/
```

![content and changes](<img/52 see content of build number.png>)

![view files](<img/53 view file.png>)


#### on the vscode

changed branch to main

```
git checkout main
```

check main branch status

```
git status
```

pull from remote repository

```
git pull
```

![git pull](<img/54 git pull.png>)


## Run  Ansible test

i connect to host folloing the image below

![connect to host](<img/55 connect to host.png>)

copied the public address of jenkins-ansible server

![get public address](<img/56 get public ip of jenkins server.png>)

ssh into development in vscode

![ssh into development](<img/57 ssh into development in vscode.png>)

choose platform

![choose platform](<img/58 choose platform.png>)

encountered error

![error](<img/59 got error choose more actions.png>)


open configuration

![open configuration](<img/60 open configuration file.png>)


![choose file path](<img/61 because i have it in my downloads folder.png>)

after opening the configurations file i type the command below

```
# Read more about SSH config files: https://linux.die.net/man/5/ssh_config
Host Jenkens-Ansible
    HostName 34.227.102.109
    User ubuntu
    IdentityFile /Users/BRIFAV-PC/Downloads/bright-usa.pem
    ForwardAgent yes
    ControlPath /tmp/ansible-ssh-%h-%p-%r
    ControlMaster auto
    ControlPersist 10m

```
![second try](<img/62 second try.png>)

#### on the jenkins-ansible server

i typed the command below to execute the playbook

```
ansible-playbook -i /var/lib/jenkins/jobs/Ansible/builds/<build_number>/archive/inventory/dev.yml /var/lib/jenkins/jobs/Ansible/builds/<build_number>/archive/playbooks/common.yml
```

![playbook executed](<img/69 playbook in action.png>)

![ansible playbook](<img/70 ansible in action.png>)


## HURRAY !!!

i check for wireshark in all the servers using the command below

```
which wireshark
```

or 

```
wireshark --version
```

![Alt text](<img/71 load balancer ssh into and wireshark.png>)

![Alt text](<img/71 load balancer ssh into.png>)

![Alt text](<img/72 nfs ssh into wireshark.png>)

![Alt text](<img/72 nfs ssh into.png>)

![Alt text](<img/73 webserver ssh into wireshark.png>)

![Alt text](<img/73 webserver ssh into.png>)

![Alt text](<img/74 logged into webserver 2 and db.png>)

![Alt text](<img/74 logged into webserver 2 wireshark.png>)

![Alt text](<img/75 logged web server 2 wireshark.png>)



<br/>
<br/>
<br/>
<br/>

### Challenges and solutions

#### JENKINS CONNECTIVITY CHALLENGE ENCOUNTERED

![ERROR](<img/jenkins/error 1.png>)

to solve the error above i adjusted the security settings using the steps below

first i visited the web directory below

```
<public-ip-address>:8080/manage
```

afterwhich i navigated to `security settings` and changed the settings as seen in the image below

```
<public-ip-address>:8080/manage/configureSecurity/
```

![corrected network connectivity issues](<img/jenkins/error issues.png>)

#### SOLVED


#### error when ssh into a new server from desktop

i use the command below

```
eval `ssh-agent -s
```

```
ssh-add <private-key>.pem
```

```
ssh -A ubuntu@<server-public-ip
```

```
ssh-add -l
```
