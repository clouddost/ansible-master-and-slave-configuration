# ansible-master-and-slave-configuration
Ansible Master and Slave / Node Configuration

# ansible-master-and-slave-configuration
Ansible Master and Slave / Node Configuration

# Installing Tomcat using Binary Method on Amazon Linux 2

## Installing Java 11 and Open JDK 11 on Amazon Linux 2:

```t
sudo amazon-linux-extras install java-openjdk11 -y
```
- Confirm the Java is installed or not using following command:

```t
java -version
```
## Install Maven

```t
sudo yum install maven -y
```
- Confirm the Maven is installed or not using following command:

```t
mvn -v
```

## Create 3 EC2 Instances 1 Controller/Master and 2 Slaves/Nodes.

* 1.	Master Instance 
* 2.	Node1 Instance
* 3.	Node2 Instance

- I used Amazon Linux 2 Instances for this hands-on.

![image](https://github.com/clouddost/ansible-master-and-slave-configuration/assets/111498842/cb03d24a-7319-43c1-8df6-b633a5184ad3)

- Connect all three instances using SSH

![image](https://github.com/clouddost/ansible-master-and-slave-configuration/assets/111498842/e361cecb-e9ac-4fd1-bb54-e66fc17c159a)

- Rename IP with hostname on all three instances

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

- Create an ansible user on Master machine:

```
useradd ansible
passwd ansible
```
### Note: The above ansible user should be sudoer / should be in the wheel group.

```
sudo usermod -aG wheel ansible
id ansible
```

### OR you can do it manually by editing sudoer file. 

``` visudo ``` OR ``` vim /etc/sudoers ``` 



