# Container Liferay 7 + tomcat + postgresql

This container has been prepared to support Liferay projets.
It can also be used for Development purpose.

## How to run it

You have to install docker-compose to run it.

Build: 
* add the jdk and liferay tomcat bundles in the asset folder
    * "liferay-7-tomcat/assets/packages/" : Oracle JDK archive (jdk-8u<...>-linux-x64.tar.gz)
    * "liferay-7-tomcat/assets/packages/" : Liferay 7.x tomcat bundle .zip archive (liferay<...>.zip)
* Simply run "docker-compose up -d"

Run: 
* afterwards can directly start/stop/restart the 2 created containers (starting with the git project name)
* Use the deploy

Connect:
* mapped ports on the local machine (remove the port mapping in the docker-compose.yml files if you want to connect directly)
* ssh to mount file system if needed ("sudo sshfs -o allow_other root@x.x.x.x:/opt/liferay /opt/liferay/docker/sshmount/ -p 22")

Adapt
* The whole tomcat configuration can be updated (Specific information for liferay container is in the liferay-7-tomcat folder. Look at the readme.md information)
    * "liferay-7-tomcat/assets/conf/portal-ext.properties" : can be updated with specific portal settings
    * "liferay-7-tomcat/assets/conf/data" : can be updated with specific files for import
    * "liferay-7-tomcat/assets/conf/tomcat/*" : can be updated with specific tomcat configurations (setenv, catalina.sh, etc...)

## features

Port mapping:
* 45432 : postresql port, user lportal/lportal for the database connexion
* 40022 : ssh connexion. "admin" is the password for the root user
* 48080 : liferay, http://localhost:48080 to go on the server
* 49000 : liferay debug port
* 41311 : Liferay Felix shell for OSGI bundles management

SSHFS mount:
* The ssh server is usefull mainly for mouting the container file system
* eg : "sudo sshfs -o allow_other root@localhost:/opt/liferay /opt/liferay/docker/sshmount/ -p 40022"

Volumes:
* "./postgres-data" : database data
* "./deploy" : liferay deploy directory for module deployment

## Tips

* Use a simple admin tool, portainer is a good one to manage containers on a local machine (https://portainer.io/)
* Specific information for liferay container is in the liferay-7-tomcat folder. Look at the readme.md information
