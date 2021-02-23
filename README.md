System update

[root@localhost ~]# sudo yum install epel-release -y

[root@localhost ~]# sudo yum update -y

[root@localhost ~]# sudo shutdown -r now


Install OpenJDK

[root@localhost ~]# java -version

[root@localhost ~]# cd /opt


Download , extarct and softlink it

Site address - https://archive.apache.org/dist/activemq/

[root@localhost opt]# wget https://archive.apache.org/dist/activemq/5.16.0/apache-activemq-5.16.0-bin.tar.gz

[root@localhost opt]# sudo tar -zxvf apache-activemq-5.16.0-bin.tar.gz -C /opt

[root@localhost opt]# ln -sf apache-activemq-5.16.0 activemq

[root@localhost opt]# ls -ltr


Start it -

[root@localhost opt]# cd activemq/

[root@localhost activemq]# nano conf/jetty.xml ( Search "jettyPort" and change localhost ip to server ip )

[root@localhost activemq]# ./bin/activemq console

[root@localhost activemq]# ./bin/activemq start

[root@localhost activemq]# ./bin/activemq stop


[root@192 activemq]# sudo systemctl start firewalld

[root@192 activemq]# sudo systemctl status firewalld

[root@localhost activemq]# sudo firewall-cmd --zone=public --permanent --add-port=8161/tcp

[root@localhost ~]# firewall-cmd --reload


Now, point your web browser to http://172.16.0.68:8161/admin and log in using the default credentials.

username: admin
password: admin

The username and password can be configured in the /opt/activemq/conf/jetty-realm.properties file.

The IP and port can be configured in the /opt/activemq/conf/jetty.xml file.

Note : Server's hostname should not have Underscore "_"
