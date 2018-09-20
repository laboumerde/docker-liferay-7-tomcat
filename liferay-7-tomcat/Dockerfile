FROM ubuntu:18.04

# openssh server
RUN apt-get update && apt-get install -y openssh-server
RUN mkdir /var/run/sshd
RUN echo 'root:admin' |chpasswd
RUN sed -ri 's/^#?PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config
RUN mkdir /root/.ssh

# supervisor installation
RUN apt-get update && apt-get -y install supervisor && \
  mkdir -p /var/log/supervisor && \
  mkdir -p /etc/supervisor/conf.d

# util apps
RUN apt-get update && apt-get -y install nano vim unzip

# Assets Copy
COPY assets /assets

# Install JDK from /assets/packages/
RUN tar -zxvf /assets/packages/jdk* -C /opt/ && \
    ln -s /opt/jdk* /opt/jdk && \
    echo "export JAVA_HOME=/opt/jdk" >> /root/.bashrc && \
    echo "export PATH=$PATH:/opt/jdk/bin" >> /root/.bashrc && \
    export JAVA_HOME=/opt/jdk && \
    export PATH=$PATH:/opt/jdk/bin

# Install Liferay from /assets/packages/
RUN unzip /assets/packages/liferay* -d /opt/ && \
    ln -s /opt/liferay* /opt/liferay && \
    ln -s /opt/liferay/tomcat* /opt/liferay/tomcat

# Configure Liferay
RUN mkdir -p /opt/liferay/deploy && \
    cp -R /assets/conf/liferay/portal-ext.properties /opt/liferay/ && \
    mkdir -p /opt/liferay/data && cp -R /assets/conf/liferay/data/* /opt/liferay/data/ && \
    cp -R /assets/conf/liferay/tomcat/* /opt/liferay/tomcat/ && \
    chmod 777 /opt/liferay/tomcat/bin/ -R

# clean
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm -rf /assets/packages

# Expose ports
EXPOSE 22
EXPOSE 8080/tcp
EXPOSE 9000/tcp
EXPOSE 11311/tcp

# Default Startup using supervisor
CMD ["supervisord", "-c", "/assets/conf/supervisor.conf"]
