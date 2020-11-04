# redhat-apache-activemq-source-installation

Step 1: Update the system

[root@localhost ~]# sudo yum install epel-release -y

[root@localhost ~]# sudo yum update -y

[root@localhost ~]# sudo shutdown -r now


Step 2: Install OpenJDK

[root@localhost ~]# java -version

[root@localhost ~]# cd /opt


Site address - https://archive.apache.org/dist/activemq/

Download , extarct and softlink it -

[root@localhost opt]# wget https://archive.apache.org/dist/activemq/5.16.0/apache-activemq-5.16.0-bin.tar.gz

[root@localhost opt]# sudo tar -zxvf apache-activemq-5.16.0-bin.tar.gz -C /opt

[root@localhost opt]# ln -sf apache-activemq-5.16.0 activemq

[root@localhost opt]# ls -ltr


Start it -

[root@localhost opt]# cd activemq/

[root@localhost activemq]# nano conf/jetty.xml ( Search "jettyPort" and change localhost ip to server ip )

[root@localhost activemq]# ./bin/activemq console

[root@localhost activemq]# ./bin/activemq start


[root@192 opt]# cd apache-activemq-5.16.0/

[root@192 apache-activemq-5.16.0]# useradd activemq

[root@192 apache-activemq-5.16.0]# chown -R activemq:activemq /opt/apache-activemq-5.16.0


Step 4: Create a Systemd unit file for Apache ActiveMQ

[root@192 apache-activemq-5.16.0]# nano /etc/systemd/system/activemq.service


[Unit]
Description=Apache ActiveMQ Message Broker
After=network-online.target

[Service]
Type=forking

User=activemq
Group=activemq

WorkingDirectory=/opt/apache-activemq-5.16.0/bin
ExecStart=/opt/apache-activemq-5.16.0/bin/activemq start
ExecStop=/opt/apache-activemq-5.16.0/bin/activemq stop
Restart=on-abort


[Install]
WantedBy=multi-user.target


[root@192 apache-activemq-5.16.0]# systemctl daemon-reload

[root@192 apache-activemq-5.16.0]# systemctl start activemq.service

[root@192 apache-activemq-5.16.0]# systemctl enable activemq.service

[root@192 apache-activemq-5.16.0]# systemctl status activemq.service


[root@localhost activemq]# sudo firewall-cmd --zone=public --permanent --add-port=8161/tcp

[root@localhost ~]# firewall-cmd --reload


Now, point your web browser to http://172.16.0.68:8161/admin and log in using the default credentials.

username: admin
password: admin

The username and password can be configured in the /opt/activemq/conf/jetty-realm.properties file.

The IP and port can be configured in the /opt/activemq/conf/jetty.xml file.





Java path activemq : 

export JAVA_HOME=/opt/java-11
PATH=${JAVA_HOME}:${JAVA_HOME}/bin:${JAVA_HOME}/lib:${MVN_HOME}:${MVN_HOME}/bin:${MVN_HOME}/lib:${ACTIVEMQ_HOME}:${ACTIVEMQ_HOME}/bin:${ACTIVEMQ_HOME}/lib:$HOME/.local/bin:$HOME/bin
