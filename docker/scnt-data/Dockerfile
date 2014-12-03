FROM centos:latest

RUN rpm -Uvh http://dev.mysql.com/get/mysql-community-release-el6-5.noarch.rpm
RUN yum -y install mysql-community-server

ADD mysql-init /usr/bin/mysql-init
RUN chmod +x /usr/bin/mysql-init
RUN /usr/bin/mysql-init

ADD mysql-start /usr/bin/mysql-start
RUN chmod +x /usr/bin/mysql-start

VOLUME ["/etc/mysql", "/var/lib/mysql"]

EXPOSE 22 3306

CMD ["/usr/bin/mysql-start"]

