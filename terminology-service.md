Install DTS
============
Follow the steps in the boxes to install DTS.  
Download the package lists from the repositories and update them to get information on the newest versions of packages and their dependencies.  
	`sudo apt-get update`    
Install ssh server to enable remote login.  
	`sudo apt-get install openssh-server`  
Open port 22 in case it is blocked by the Ubuntu firewall    
	`sudo ufw allow 22`    
Move to the home directory and download DTS.     
	`cd /home/dts`
	`wget http://sourceforge.net/projects/apelon-dts/files/dts-linux/3.5.2/apelon-dts-3.5.2.203-linux-mysql.tar.gz/download`   
Create a directory “apelon-dts-3.5-linux” under  “/opt”   
	`sudo mkdir /opt/apelon-dts-3.5-linux`   
Go to the apelon-dts-3.5-linux directory and move the file you downloaded into the dts home directory.   
	`cd /opt/apelon-dts-3.5-linux`   
	`sudo mv  /home/dts/download apelon-dts-3.5.2.203-linux-mysql.tar.gz`   
Unzip the file:   
`sudo tar -zxf apelon-dts-3.5.2.203-linux-mysql.tar.gz`   
Move to the bin directory   
	`cd bin`   
Make all the DTS scripts in the bin directory executable   
	`sudo sh makeScriptsExecutable.sh`    
Install MySQL    
`sudo apt-get install mysql-server`   
Login to mysql as root and create a user “dts”   
	`mysql –u root –p`   
In response to the password prompt, enter the root password   
	`create user 'dts'@'localhost' identified by 'dts';
grant all privileges on *.* to 'dts'@'localhost' with grant option;
exit`   


Edit the MySQL config file /etc/mysql/my.conf   
	`sudo vi /etc/mysql/my.cnf`   
and add the following lines under the section titled [mysqld]   

	lower_case_table_names=1 
	default-storage-engine=InnoDB
	max_allowed_packet=100M
	character-set-server=utf8
	collation-server=utf8_general_ci   

Stop and restart MySQL server   
	`sudo /etc/init.d/mysql stop
	sudo /etc/init.d/mysql start`   

Create the DTS schema using the schema creation tool:   

In the /opt/apelon-dts-3.5-linux/bin/kb directory, edit the files named source-connection.xml and target-connection.xml, and ensure that the connection properties are setup for mysql   
  
  	<!--
	  MySQL connection.
	  -->
	 <connection>
	  <property name="direction" value="source"/>
	  <property name="type" value="mysql"/>
	  <property name="user" value="dts"/>
	 <property name="pass" value="dts"/>
	 <property name="host" value="localhost"/>
	 <property name="databaseName" value="dts"/>
	  <property name="databasePort" value="3306"/>
	 <property name="jdbcDriver" value="com.mysql.jdbc.Driver"/>
	 <property name="url_template" value="jdbc:mysql://[HOST]:[PORT]/[DATABASE]"/>
	 <property name="blockSize" value="256"/>
  	</connection> 

Download the package lists from the repositories and update them to get information on the newest versions of packages and their dependencies once again.   
`sudo apt-get update`   
Install Java    
`sudo apt-get install openjdk-7-jre`   

Ensure you are in the /opt/apelon-dts-3.5-linux/bin directory and then start the dts server   
	`cd /opt/apelon-dts-3.5-linux/bin`   
`sudo ./dts.sh start`   

The DTS server should now be up.   
