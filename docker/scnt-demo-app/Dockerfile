FROM centos:latest

#Setting up ssh key to clone a private repo
RUN mkdir -p /root/.ssh
ADD /package/id_rsa /root/.ssh/id_rsa
ADD /package/settings.xml /root/.m2/

ADD /package/openam_policy_agents /usr/local/openam_policy_agents/

RUN chmod 600 /root/.ssh/id_rsa
RUN chown -R root:root /root/.ssh
RUN echo "Host github.com\n\tStrictHostKeyChecking no\n" >> /root/.ssh/config

#Create known hosts
RUN touch /root/.ssh/known_hosts
RUN ssh-keyscan github.com >> /root/.ssh/known_hosts

#Clone the application itself
RUN cd /usr/local && git clone -b PSI1-prototype git@github.com:ScholasticInc/scent-svc-core.git

#Build the app once, so we can include all the dependencies in the image
RUN mv /usr/local/scent-svc-core/scent-prototype/scent-prototype-web/src/main/resources/mysql-config.xml /usr/local/scent-svc-core/scent-prototype/scent-prototype-web/src/main/resources/dev-db-config-old.xml
ADD /package/mysql-config.xml /usr/local/scent-svc-core/scent-prototype/scent-prototype-web/src/main/resources/
RUN cd /usr/local/scent-svc-core/scent-prototype/scent-prototype-web && /usr/local/maven/bin/mvn -Dmaven.test.skip=true package -X

RUN cp /usr/local/scent-svc-core/scent-prototype/scent-prototype-web/target/scent-prototype-web.war $JBOSS_HOME/standalone/deployments/
ADD /package/agentapp.war /usr/local/jboss-as/standalone/deployments/
RUN mv /usr/local/jboss-as/standalone/configuration/standalone.xml /usr/local/jboss-as/standalone/configuration/standalone-backup.xml
ADD /package/standalone.xml /usr/local/jboss-as/standalone/configuration/
ADD /package/forgerock /usr/local/jboss-as/modules/org/forgerock

EXPOSE 8081

CMD ["/usr/local/jboss-as/bin/standalone.sh", "-b", "0.0.0.0", "-Djboss.http.port=8081"]


