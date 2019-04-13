
# MongoDB_pymongo_GCP  
Install MongoDB on Google Clound Platform(GCP) then mainpulation with pymongo  

# Table of Contents
[Implementation](#implementation)  
[Step 1](#step-1)  
[Import public key for package managemnet systerm](#import-public-key-for-package-managemnet-systerm)  
[Step 2](#step-2)  
[Establish a list file](#establish-a-list-file)  
[Step 3](#step-3)  
[Step 4](#step-4)  
[Install MongoDB](#install-mongodb)  
[Specify MongoDB version](#specify-mongodb-version)  
[Fix MongoDB version](#fix-mongodb-version)  
[Step 5](#step-5)  
[Excute MongoDB](#excute-mongodb)  
[Verify MongoDB opertaion to check content of /var/log/mongodb/mongod.log](#verify-mongodb-opertaion-to-check-content-of-varlogmongodbmongodlog)  
[Stop MongoDB](#stop-mongodb)  
[Restart MongoDB](#restart-mongodb)  
[Step 6](#step-6)  
[Open up mongo shell](#open-up-mongo-shell)  
[Inside mongo shell access the admin database. Create a new admin user.](#inside-mongo-shell-access-the-admin-database-create-a-new-admin-user)  
[Step 7](#step-7)  
[Enable MongoDB Auth and open MongoDB access up to all IPs](#enable-mongodb-auth-and-open-mongodb-access-up-to-all-ips)  
[Restart MongoDB](#restart-mongodb-1)  
[Try to access the mongo shell by simply typing mongo in the terminal](#try-to-access-the-mongo-shell-by-simply-typing-mongo-in-the-terminal)  
[Enable MongoDB after Reboot](#enable-mongodb-after-reboot)  
[Step 8](#step-8)  
[Edit firewall rule to allow port 27017 to access MongoDB](#edit-firewall-rule-to-allow-port-27017-to-access-mongodb)  
[Step 9](#step-9)  
[Connect to your remote MongoDB server](#connect-to-your-remote-mongodb-server)  
[Test connection to your remote MongoDB server](#test-connection-to-your-remote-mongodb-server)  
[Press "!Test" buttion to test connection](#press-test-buttion-to-test-connection)  
[Step 10](#step-10)  
[First,](#first)  
[Under python console](#under-python-console)  

[Troubleshooting](#troubleshooting)  
[Environment Configuration](#environment-configuration)  
[Reference](#reference)  

# Implementation  
* [在Ubuntu 16.04上安裝、使用、解除安裝MongoDB](https://www.itread01.com/content/1545355444.html)
* [Setting up and connecting to a remote MongoDB database](https://medium.com/founding-ithaka/setting-up-and-connecting-to-a-remote-mongodb-database-5df754a4da89)
* [How to connect to your remote MongoDB server](https://ianlondon.github.io/blog/mongodb-auth/)


## Step 1  
### Import public key for package managemnet systerm    

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2930ADAE8CAF5059EE73BB4B58712A2291FA4AD5
``` 

## Step 2  
### Establish a list file    

Ubuntu 16.04
``` 
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.6 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.6.list
``` 

## Step 3  
``` 
sudo apt-get update
``` 

## Step 4   
### Install MongoDB  
``` 
sudo apt-get install -y mongodb-org
``` 

### Specify MongoDB version  
``` 
sudo apt-get install -y mongodb-org=3.6.0 mongodb-org-server=3.6.0 mongodb-org-shell=3.6.0 mongodb-org-mongos=3.6.0 mongodb-org-tools=3.6.0
``` 

### Fix MongoDB version  
``` 
echo "mongodb-org hold" | sudo dpkg --set-selections
echo "mongodb-org-server hold" | sudo dpkg --set-selections
echo "mongodb-org-shell hold" | sudo dpkg --set-selections
echo "mongodb-org-mongos hold" | sudo dpkg --set-selections
echo "mongodb-org-tools hold" | sudo dpkg --set-selections
``` 

## Step 5  
### Excute MongoDB  

``` 
sudo service mongod start
``` 

### Verify MongoDB opertaion to check content of /var/log/mongodb/mongod.log  
``` 
[initandlisten] waiting for connections on port 27017
``` 

### Stop MongoDB  
``` 
sudo service mongod stop
``` 

### Restart MongoDB  
``` 
sudo service mongod restart
``` 

## Step 6  
### Setup database users and assign them roles  

### Open up mongo shell  
``` 
$ mongo
``` 

### Inside mongo shell access the admin database. Create a new admin user.  
``` 
> use admin;
> db.createUser({
      user: "admin",
      pwd: "myadminpassword",
      roles: [
                { role: "userAdminAnyDatabase", db: "admin" },
                { role: "readWriteAnyDatabase", db: "admin" },
                { role: "dbAdminAnyDatabase",   db: "admin" }
             ]
  });
``` 

## Step 7  
### Enable MongoDB Auth and open MongoDB access up to all IPs  

> Once your admin is setup and database specific users have been created, it is now time to enable MongoDB to start using these access controls.   

sudo nano /etc/mongod.conf  
``` 
# network interfaces
net:
    port: 27017
    bindIp: 0.0.0.0   #default value is 127.0.0.1
``` 

``` 
security:
    authorization: 'enabled'
``` 

### Restart MongoDB  

``` 
sudo service mongod restart
``` 

### Try to access the mongo shell by simply typing mongo in the terminal  
``` 
# to access the admin database
ubuntu:~$ mongo -u admin -p myadminpassword 127.0.0.1/admin
``` 

### Enable MongoDB after Reboot  
```  	
$ sudo systemctl enable mongod
``` 


## Step 8  
### Edit firewall rule to allow port 27017 to access MongoDB  
> VPC network---> Firewall rules---> Create Firewall rule---> Edit---> Save

![alt tag](https://i.imgur.com/iu9G0sr.jpg)

## Step 9  
### Connect to your remote MongoDB server  

[Download Robo 3T (formerly Robomongo)](https://robomongo.org/)

### Test connection to your remote MongoDB server  
![alt tag](https://i.imgur.com/tMPuuYc.jpg)  
![alt tag](https://i.imgur.com/FvEJCVu.jpg)  

### Press "!Test" buttion to test connection  
![alt tag](https://i.imgur.com/KR3FWjH.jpg)  

## Step 10  
### Using pymongo to connect with your remote MongoDB server  

### First,  
``` 
pip install pymongo
``` 
### Under python console  
![alt tag](https://i.imgur.com/6kB4uKn.jpg)


# Troubleshooting  

# Environment Configuration  
``` 
o Ubuntu 16.04.5 LTS (GNU/Linux 4.15.0-1026-gcp x86_64)。
o MongoDB 3.6。
o Robo 3T 1.2.1。
o Python 3.6。
``` 


# Reference   
* [Install MongoDB Community Edition on Ubuntu](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/#install-mongodb-community-edition)
* [MongoDB (1) 安装（apt-get）及使用(4.0.0版)](https://konfido.github.io/2018/07/13/MongoDB-1-install/)
* [MongoDB (2) 创建用户及密码(授权)](https://konfido.github.io/2018/07/14/MongoDB-2-auth/)
* [MongoDB (3) 各种错误解决方法](https://konfido.github.io/2018/07/14/MongoDB-3-errors/)
* [在Ubuntu 16.04上安裝、使用、解除安裝MongoDB](https://www.itread01.com/content/1545355444.html)
* [Running mongodb on ubuntu 16.04 LTS](https://stackoverflow.com/questions/37014186/running-mongodb-on-ubuntu-16-04-lts)
* [[Linux] MongoDB 與 PyMongo 初體驗 @ Ubuntu 12.04, Linode](http://blog.changyy.org/2014/01/linux-mongodb-pymongo-ubuntu-1204-linode.html)
* [Setting up and connecting to a remote MongoDB database](https://medium.com/founding-ithaka/setting-up-and-connecting-to-a-remote-mongodb-database-5df754a4da89)
* [How to connect to your remote MongoDB server](https://ianlondon.github.io/blog/mongodb-auth/)
* [Security](https://docs.mongodb.com/manual/security/#SecurityandAuthentication-Ports)

* [mongodb group by](https://mlwmlw.org/2015/03/mongodb-group-by/)
* [MongoDB 透過 lookup pipeline 實踐 Left Join](https://mlwmlw.org/2018/10/mongodb-left-join/#more-2983)
* [迎接大數據來臨！MongoDB 操作實錄](https://hkitblog.com/mongodb-%E8%BF%8E%E6%8E%A5%E5%A4%A7%E6%95%B8%E6%93%9A%E4%BE%86%E8%87%A8%EF%BC%81/)
* [python+mongoDB = pymongo教學](http://rasca0027.logdown.com/posts/252512-python-mongodb-pymongo-teaching)

* [Robomongo — 好用的 MongoDB GUI manager](https://medium.com/@wilsonhuang/robomongo-%E5%A5%BD%E7%94%A8%E7%9A%84-mongodb-gui-manager-87508da806e5)
* [Taking a Look at Robomongo and Studio 3T with Compose for MongoDB](https://www.compose.com/articles/taking-a-look-at-robomongo-and-studio-3t-with-compose-for-mongodb/)

* [Ubuntu16.04 中 mongodb 怎么设置开机启动](https://blog.csdn.net/a727911438/article/details/80464124)
* [Ubuntu 16.04 安装 MongoDB 开机自启](http://blog.topspeedsnail.com/archives/4820)

* [Tclunqlite v0.3.3 2018-01-23](http://tcl-eval.blogspot.com/2018/01/)

* []()
![alt tag]()