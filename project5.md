## Implement a Client Server Architecture using MySQL Database Management System (DBMS) ##


**Step 1**

Create and configure two Linux-based virtual servers (EC2 instances in AWS).

Server A name - `mysql server`
Server B name - `mysql client`

![Clientand server](./images/client%20and%20server.jpg)

Run update on both the mysql client and server

`sudo apt update`
![update mysql client & server](./images/apt%20update%20client.jpg)


On mysql-server, install MySQL Server software.

`sudo apt install mysql-server`
![install mysql-server](./images/install%20mysql-server.jpg)

On mysql-client, install MySQL client software

`sudo intall mysql-client`

![install mqsql-client](./images/install%20mysql-client.jpg)

By default, both of your EC2 virtual servers are located in the same local virtual network, so they can communicate to each other using local IP addresses. Use mysql server's local IP address to connect from mysql client. MySQL server uses TCP port 3306 by default, so you will have to open it by creating a new entry in ‘Inbound rules’ in ‘mysql server’ Security Groups.

![lock down port 3306](./images/allow%20port%203306%20on%20the%20inbound%20rule%20of%20mysql-server's%20security%20group.jpg)

Configure MySQL server to allow connections from remote hosts.

`sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf`

Replace ‘127.0.0.1’ to ‘0.0.0.0’ 

![allow remote connections](./images/configure%20MySQL%20server%20to%20allow%20connections%20from%20remote%20hosts.jpg)

## From mysql client Linux Server connect remotely to mysql server Database Engine using the mysql utility. ##


1.  Log in to the MySQL console in mysql-server by typing

    `sudo mysql`

    ![mysql](./images/connect%20to%20my%20sql.jpg)

2. Remove some insecure default settings and lock down access to your database system

    `ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';`

    ![CREATE PASSWORD FOr the root user](./images/remove%20insecure%20settings.jpg)


3. Exit the MySQL shell 

    `exit`

    ![exit](./images/exit.jpg)

4. Create a new database from MySQL console

    `CREATE DATABASE `example_database`;`

    ![Create a new database from MySQL console](./images/create_database.jpg)


5. Creates a new user named example_user, using mysql_native_password as default

    `CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';`

    ![creates a new user named example_user, using mysql_native_password as default](./images/creates%20new%20use%20with%20password%20policy.jpg)

6. Give this user permission over the example_database database, then exit

    `GRANT ALL ON example_database.* TO 'example_user'@'%';`

    ![Give this user permission over the example_database database](./images/give%20the%20user%20permissions%20on%20the%20example_database.jpg)

7. Test new user's permissions*

    `mysql -u example_user -p`

    ![Test new user's permissions](./images/test-new-users-permission.jpg)

8. Connect remotely to mysql-server from mysql-client and show databases

    `mysql -u example_user -h 172.31.13.110 -p`
    `Show databases`

    ![Connect remotely and show databases](./images/Connect%20remotely%20with%20example_user%20and%20showing%20databases.jp