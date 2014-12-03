#Use latest Fedora image as the base
FROM centos:latest

# Update base image
#RUN yum -y update && yum clean all

#Install jdk 7
ADD ./jdk /tmp
RUN rpm -i /tmp/jdk-7u71-linux-x64.rpm
RUN rm /tmp/jdk-7u71-linux-x64.rpm

#Download and Install git,wget and tar
RUN yum -y --noplugins --verbose update
RUN yum -y --noplugins --verbose install nginx git wget tar unzip

#Download JBoss
RUN wget http://download.jboss.org/jbossas/7.1/jboss-as-7.1.1.Final/jboss-as-7.1.1.Final.zip
RUN unzip jboss-as-7.1.1.Final.zip -d /usr/local
RUN ln -s /usr/local/jboss-as-7.1.1.Final /usr/local/jboss-as

#Download and Install Maven
ADD ./maven /tmp
RUN cd /usr/local && tar xzf /tmp/apache-maven-3.2.3-bin.tar.gz
RUN ln -s /usr/local/apache-maven-3.2.3 /usr/local/maven
RUN rm /tmp/apache-maven-3.2.3-bin.tar.gz

#ENVIRONMENT VARIABLES
ENV JBOSS_HOME /usr/local/jboss-as
ENV MAVEN_HOME /usr/local/maven
ENV JAVA_HOME /usr/java/latest


