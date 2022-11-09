# Install MariaDB 10.5 on CentOS7

## Tools and Technologies Used:
- VM --> Aws Ec2
- Centos7
- MariaDb 10.5
- MobaXterm
-----------
### İnstallation:

First of all, we start by creating an instance on Aws Ec2.We can look at the video below for a sample instance installation:


https://user-images.githubusercontent.com/64022432/200653381-87315e4c-7e08-4cf1-87d2-d8875da014c0.mp4

------------
- Afterwards, we reach our machine via MobaXterm by specifying the Public ip of the created Instance and the file location of the ssh key we created as seen in the video. For me, this command:
>ssh -i C:/aws/firstkey.pem centos@3.84.179.75
-----------------------------
### MariaDb installation:

- Let's update Centos first:
> sudo yum update

- Run the following commands to add the repository provided by MariaDB to your CentOS server:
> curl -LsS -O https://downloads.mariadb.com/MariaDB/mariadb_repo_setup
> sudo bash mariadb_repo_setup --mariadb-server-version=10.5

- Confirm the repository is working by updating cache:
> sudo yum makecache

- List available repositories:
> sudo yum repolist
> ![sudo yum repolist](https://user-images.githubusercontent.com/64022432/200656148-6fc87c79-5305-4ee0-b49b-e6a6e583e62c.jpg)

### Let's continue with the installation:

> sudo yum install MariaDB-server MariaDB-client MariaDB-backup

- You can check list of packages to be installed and agree if okay with it:

### RPM package details:

> rpm -qi  MariaDB-server 
> ![rpm -qi  MariaDB-server](https://user-images.githubusercontent.com/64022432/200657251-e85aa1b6-2c0f-4b7e-a03b-0ca6b2328143.jpg)

### CentOS 7 uses Systemd init system. We can start the service with systemctl command as shown below:

> sudo systemctl start mariadb

- To enable the service to be started when the server is rebooted use the following command:

> sudo systemctl enable mariadb

### Check service status with systemctl status command:
> systemctl status mariadb
> ![systemctl status mariadb](https://user-images.githubusercontent.com/64022432/200657927-01323d3d-f24d-4a47-b103-661f2f96df2f.jpg)


- The following command is required to secure MariaDB. This script program will ask various questions and in some places it will ask you to enter the password:

> sudo mariadb-secure-installation
> ![sudo mariadb-secure-installation](https://user-images.githubusercontent.com/64022432/200658664-5ea31412-3500-43ae-9d15-dc3d76e4caba.jpg)

- Let's connect to MariaDB's console:

> mariadb -u root -p
> ![mysql -u root -p](https://user-images.githubusercontent.com/64022432/200658976-15b3d2d5-6c34-436b-897a-f8d6700d26fe.jpg)

- Our database server is now ready for use


## Creating a sample database, table and user and adding a specific privilege to this user:

- Database Creation:

> CREATE DATABASE <Your_db>;
> ![create databases](https://user-images.githubusercontent.com/64022432/200659759-09ac315e-00d2-4735-bf81-b527b55602b5.jpg

- Creating a Table:

> CREATE TABLE interviews (
> id int not null auto_increment,
> name varchar(255),
> primary key (id)
> );


> ![create table interviews](https://user-images.githubusercontent.com/64022432/200660463-fb1b507d-e432-401e-be0c-18a8aca1573c.jpg)
---------------------
- Create user with specific privilege:


> ![user2 create](https://user-images.githubusercontent.com/64022432/200660853-c9be46af-cddc-4247-ae8e-15a60d95d7e9.jpg)

### Here I chose SELECT as privilege.Explanations for SELECT and other privileges

- CREATE – Allows users to create databases and tables
- SELECT – Allows users to read data
- INSERT – Allows users to add new data to tables
- UPDATE – Allows users to update existing data in their tables
- DELETE – Allows users to delete table data
- DROP – Allows users to delete entire database or tables

### Finally
### After logging out with the Quit command, let's log in with the user we just created and examine the changes we have made:


> ![describe user2](https://user-images.githubusercontent.com/64022432/200662615-7a7a152c-bd41-4ff7-bd0e-f9c54004af73.jpg)


> ![grant show for user2](https://user-images.githubusercontent.com/64022432/200662036-7af91f8d-9212-4198-81ca-2dc7b725bd40.jpg)

