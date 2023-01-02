# TOMCAT-SETUP ON UBUNTU
## Introduction :
* Apache Tomcat is an open-source, lightweight web server for running Java-based websites and applications. It's an implementation of the Jakarta EE platform which is the modification of the Java EE platform to accommodate distributed computing and web services.

* First update the repo :
```bash
 sudo apt update
```
* Next we need to install the any java or jdk ,,to serach jdk all versions :
```bash
 sudo apt search jdk -y
```
* Install the jdk :
```bash
 sudo apt-get install openjdk-8-jdk -y
```
* To see the version of jdk :
```bash
 java -version
```
* Next go to /opt/ directory :
```bash
 cd /opt/
```
* Next download the tomcat from browser with wget command :
```bash
 wget https://archive.apache.org/dist/tomcat/tomcat-10/v10.0.8/bin/apache-tomcat-10.0.8.tar.gz
```
* Extract the download archive :
```bash
 sudo tar xzvf apache-tomcat-10.0.8.tar.gz
```
* Rename the archived file into tomcat :
```bash
 sudo mv apache-tomcat-10.0.8/* /opt/tomcat/ 
```
* Change ownership of that directory :
```bash
 sudo chown -R www-data:www-data /opt/tomcat/
```
* Change access permissions for that directory :
```bash
  sudo chmod -R 755 /opt/tomcat/
```
* Edit /opt/tomcat/conf/tomcat-users.xml file to configure an administrator and manager account for Apache Tomcat :
```bash
 sudo vim /opt/tomcat/conf/tomcat-users.xml
```
* Add the code below within the <tomcat-users> tag. Change the password for administrator and manager access by changing the value StrongPassword below with a high secure password :
```bash
 <!-- user manager can access only manager section -->

<role rolename="manager-gui" />

<user username="manager" password="StrongPassword" roles="manager-gui" />



<!-- user admin can access manager and admin section both -->

<role rolename="admin-gui" />

<user username="admin" password="StrongPassword" roles="manager-gui,admin-gui" />
```
* Enable remote access to Apache Tomcat by editing manager and host-manager configuration files. Edit manager application context.xml file :
```bash
 sudo vim /opt/tomcat/webapps/manager/META-INF/context.xml
```
* Comment out the IP addresses section as shown below. Then, save and close the file :
``` bash
 <!-- <Valve className="org.apache.catalina.valves.RemoteAddrValve"

         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> -->
```
* Edit host manager application context.xml file :
```bash
 sudo vim /opt/tomcat/webapps/host-manager/META-INF/context.xml
```
* Comment out the IP addresses section as shown below. Then, save and close the file :
```bash
 <!--<Valve className="org.apache.catalina.valves.RemoteAddrValve"

         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> -->
```
* Create a systemd unit file for Apache Tomcat :
```bash
 sudo vim /etc/systemd/system/tomcat.service
```
* Add the code below to the file. Then, save and close the file :
```bash
 [Unit]

Description=Tomcat

After=network.target



[Service]

Type=forking



User=root

Group=root



Environment="JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64"

Environment="JAVA_OPTS=-Djava.security.egd=file:///dev/urandom"

Environment="CATALINA_BASE=/opt/tomcat"

Environment="CATALINA_HOME=/opt/tomcat"

Environment="CATALINA_PID=/opt/tomcat/temp/tomcat.pid"

Environment="CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC"



ExecStart=/opt/tomcat/bin/startup.sh

ExecStop=/opt/tomcat/bin/shutdown.sh



[Install]

WantedBy=multi-user.target
```
* Reload the systemd daemon service to apply changes :
```bash
  sudo systemctl daemon-reload
```
* Start Apache Tomcat service :
```bash
 sudo systemctl start tomcat
```
* Enable the service to start up on system boot :
```bash
  sudo systemctl enable tomcat
```
* Check the status of the service.
```bash
 sudo systemctl status tomcat
```
* Access Apache Tomcat Web Interface :
```bash
 https://publicip:8080
```

## Tomcat interface looks like this..

![App Screenshot](https://www.how2shout.com/linux/wp-content/uploads/2022/06/Access-Tomcat-Web-interface.png)



