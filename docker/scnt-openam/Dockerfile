FROM centos:latest

#set up
#Install jdk 7
ADD ./package /tmp
RUN rpm -i /tmp/jdk-7u71-linux-x64.rpm
RUN rm /tmp/jdk-7u71-linux-x64.rpm

#Download and Install git,wget and tar
RUN yum -y --noplugins --verbose update
RUN yum -y --noplugins --verbose install nginx git wget tar unzip

RUN wget -O /tmp/apache-tomcat-7.0.57.tar.gz  http://www.us.apache.org/dist/tomcat/tomcat-7/v7.0.57/bin/apache-tomcat-7.0.57.tar.gz
RUN cd /tmp && tar xzf apache-tomcat-7.0.57.tar.gz
RUN mv /tmp/apache-tomcat-7.0.57 /usr/share/tomcat7

#setup httpd
RUN yum install -y httpd mod_ssl
ADD /package/openam-proxy.conf /etc/httpd/conf.d/openam-proxy.conf

#RUN curl http://download.forgerock.org/downloads/openam/openam_link.js | grep -o "http://.*\.war" | xargs curl -o /usr/share/tomcat7/webapps/openam.war
ADD ./package/openam.war /usr/share/tomcat7/webapps/
ADD ./package/openam /usr/share/tomcat7/webapps/
RUN chmod -R 755 /usr/share/tomcat7

ENV CATALINA_HOME /usr/share/tomcat7
ENV PATH $CATALINA_HOME/bin:$PATH
ENV CATALINA_OPTS -Xms512M -Xmx2048M
ENV PATH $CATALINA_OPTS:$PATH

#Run tomcat
CMD $CATALINA_HOME/bin/catalina.sh start && wait && /usr/sbin/httpd && wait && tail -f /usr/share/tomcat7/logs/catalina.out

