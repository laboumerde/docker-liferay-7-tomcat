## Docker image for Liferay DXP

The project will build a docker image with up and running Liferay 7. The image include :

* JDK installation from provided tar.gz  

* Liferay installation from provided liferay<...>.zip bundle with tomcat server.

**Note :** The docker image should not be used in production environment.

## Required Customization

Before running the docker builder, you need to provide following packages :

* "./assets/packages/" : Oracle JDK archive (jdk-8u<...>-linux-x64.tar.gz)

* "./assets/packages/" : Liferay 7.x tomcat bundle .zip archive (liferay<...>.zip)

License and default modules :

* you will need to create a folder (in our example "/opt/liferay/docker/deploy") to be able to link a host deploy folder to the one in the container (used in the docker run command below).

* Then you will be able to copy license and other liferay modules by placing them on the "/opt/liferay/docker/deploy" folder

## Build & Run & Start

From project root folder :

    docker build -t liferay-7-tomcat .
    JOB=$(docker run -d -v /opt/liferay/docker/deploy:/opt/liferay/deploy --name liferay-7-tomcat-x liferay-7-tomcat)
    docker start $JOB

Note :

    Arbitrary image and container name : liferay-7-tomcat
    Exposed ports :
        port 22 (ssh)
        port 8080 (tomcat)
        port 9000 (tomcat debug port)
        port 11311 (OSGI felix remote port)   

## Accounts :

System account :

    user : root
    pwd : admin

## Access to Liferay Portal :

Open **http://x.x.x.x:8080** in the browser of your choice and finish portal initialization.

**Note :** Please note that the first Liferay startup can take up to few minutes.

## Obvious Tips

* For those who want to mount liferay server on local system

    sudo sshfs -o allow_other root@x.x.x.x:/opt/liferay /opt/liferay/docker/sshmount/ -p 22
