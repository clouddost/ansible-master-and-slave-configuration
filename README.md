# Ansible Master and Slave / Node Configuration

## Create 3 EC2 Instances 1 Controller/Master and 2 Slaves/Nodes.

* Master Instance 
* Node1 Instance
* Node2 Instance

## I used Amazon Linux 2 Instances for this hands-on.

![image](https://github.com/clouddost/ansible-master-and-slave-configuration/assets/111498842/cb03d24a-7319-43c1-8df6-b633a5184ad3)

## Connect all three instances using SSH Client

![image](https://github.com/clouddost/ansible-master-and-slave-configuration/assets/111498842/e361cecb-e9ac-4fd1-bb54-e66fc17c159a)

## Rename IP with hostname on all three instances

Master Instance
```
sudo hostnamectl set-hostname master
```
Node1 Instance
```
sudo hostnamectl set-hostname node1
```
Node2 Instance
```
sudo hostnamectl set-hostname node2
```

## Create an ansible user on Master machine:

```
useradd ansible
passwd ansible
```
### Note: The above ansible user should be sudoer / should be in the wheel group.

```
sudo usermod -aG wheel ansible
id ansible
```
#### OR you can do it manually by editing sudoer file. 

```visudo``` OR ```vim /etc/sudoers``` 

Replace ``` # %wheel ``` with ``` ansible ```

```
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL
```
```
## Same thing without a password
ansible        ALL=(ALL)       NOPASSWD: ALL
```
![image](https://github.com/clouddost/ansible-master-and-slave-configuration/assets/111498842/b807ab21-0fcf-45b0-9218-cc2beabaffdb)
```
id ansible
```
### Master: Enable password authentication, it must ask the password while exchanging the ssh-keys in the ssh configuration file.

```
vim /etc/ssh/sshd_config
```
```
# To disable tunneled clear text passwords, change to no here!
#PasswordAuthentication yes uncomment this
#PermitEmptyPasswords no
```
![image](https://github.com/clouddost/ansible-master-and-slave-configuration/assets/111498842/51c77f87-5bd8-4fde-8c31-b12f99aaa51f)

```
service sshd status
service sshd restart
```
### Install Ansible on Master.

https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html

### Setup Master server:

Make sure python and pip is installed.

``` python –version ``` OR ``` python3 --version ```
``` pip --version ``` OR ``` pip3 --version ```

Install pip if not installed

```yum install pip -y``` OR ```yum install pip3 -y```

Installing ansible
```
python3 -m pip install --user ansible
ansible --version
```
### Setup client/node machines - as root user
Ansible is agentless, so not required to install on client/node machines.
```
python3 --version
```
```
useradd ansible
passwd ansible
```
Do this on both the nodes, also make sure they are sudoers.
Nodes: Enable password authentication, it must ask the password while exchanging the ssh-keys in the ssh configuration file.

```
vim /etc/ssh/sshd_config
```
```
# To disable tunneled clear text passwords, change to no here!
#PasswordAuthentication yes uncomment this
#PermitEmptyPasswords no
```
```
service sshd status
service sshd restart
```
### On Master Machine: As ansible user

```sudo su - ansible```
```whoami```
```pwd``` Make sure directory should be /home/ansible
```ssh-keygen``` Generate ssh key on Master

Copy Public key (id_rsa.pub) to client machine.

```
ssh-copy-id ansible@<public-ip OR private-ip/>
```
```ssh-copy-id ansible@172.31.87.142``` Node1
```ssh-copy-id ansible@172.31.93.18``` Node2

Now try logging into the machine, with: 
```ssh ansible@172.31.87.142`` Node2
![image](https://github.com/clouddost/ansible-master-and-slave-configuration/assets/111498842/becd19d7-6bf7-4adc-a0e2-9feb271f23d8)

```ssh ansible@172.31.93.18``` Node2
![image](https://github.com/clouddost/ansible-master-and-slave-configuration/assets/111498842/47d7e291-cd8a-4e28-bc76-a91b7bb6e7fd)

Make sure you have ansible.cfg and inventory/hosts files

```ansible.cfg``` file
```
[defaults]
inventory = hosts
```
![image](https://github.com/clouddost/ansible-master-and-slave-configuration/assets/111498842/d11ecaa1-825f-48ba-b590-26c047ea3760)

```hosts``` file

![image](https://github.com/clouddost/ansible-master-and-slave-configuration/assets/111498842/698b0d22-e093-49af-b632-47cb285ef3e7)

- Talk to Client Nodes from Master Machine

```
ansible all -m ping
```
![image](https://github.com/clouddost/ansible-master-and-slave-configuration/assets/111498842/88296430-ed74-4914-9194-80f5e0a50e75)

![image](https://github.com/clouddost/ansible-master-and-slave-configuration/assets/111498842/b7e57f96-2cfc-400c-b4d6-bc255028cae5)






