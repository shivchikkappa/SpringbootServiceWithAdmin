# Introduction
This tutorial will walk you through the process of creating springboot microservices and register them to springboot admin for monitoring the application. The deployment will be done using docker run commands and docker compose. 

The tutorial assumes that users have basic knowledge of springboot, springboot admin and docker.

1. [Build docker images](#build-docker-images)
2. [Create docker network](#create-docker-network)
3. [Start docker containers](#start-docker-containers)
4. [Docker command options explained](#docker-command-options-explained)
5. [Docker compose solution](#docker-compose-solution)

## Build docker images

1. Clone the repo from github
2. cd springbootadmin and follow the [README.md](https://github.com/shivchikkappa/SpringbootServiceWithAdmin/blob/master/springbootadmin/README.md) to build the docker image 
3. cd DemoApplication and follow the instuctions from [README.md](https://github.com/shivchikkappa/SpringbootServiceWithAdmin/blob/master/DemoApplication/README.md) to build the docker image.

## Create docker network

1. Create the docker network named "springboot-admin".
```bash
docker network create springboot-admin
```
2. Confirm docker network is created.
```bash
docker network ls
```
3. Use the inspect command to check the docker network. At this moment, the network will not show any 
```bash
docker network inspect springboot-admin
```
## Start docker containers

1. Start the springboot admin associated with "sprignboot-admin" network using option "--network springboot-admin". The value used with --name should be the value used in springboot microservice [application.properties](https://github.com/shivchikkappa/SpringbootServiceWithAdmin/blob/master/DemoApplication/src/main/resources/application.properties) property spring.boot.admin.client.url
  
```bash
docker run -d --restart unless-stopped --network springboot-admin --name myspringbootadmin -e spring_boot_admin_notify_mail_from=from@domainname -p 9060:9060 -p 9065:9065 myspringbootadmin
```
2. Start the demo application running on port 8080 associated with "sprignboot-admin" network using option "--network springboot-admin". Note that default application name is overridden as "services-API-Dev"
```bash
docker run -d --restart unless-stopped --network springboot-admin --name mydemoapplication -e SPRING_APPLICATION_NAME=Service-API-Dev -p 8080:9080 -p 8085:9085 mydemoapplication
```
3. Confirm both applications are listed in docker network
```bash
docker network inspect springboot-admin
[
    {
        "Name": "springboot-admin",
        "Id": "c60f36501ad67409d501bfd69c3ad2781fb8a7cfe3644bb4c468af6dc4b2d7e0",
        "Created": "2020-09-29T14:24:22.351477901Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.20.0.0/16",
                    "Gateway": "172.20.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "7a3b3510921c33794cb1de87a0ac957cc6c3639f662b1eacb865ef6787dc0007": {
                "Name": "mydemoapplication",
                "EndpointID": "8de82b3368cb8d3267dad08ba9f58ac1c1c305bc41ef779c9d145a11721e7227",
                "MacAddress": "02:42:ac:14:00:03",
                "IPv4Address": "172.20.0.3/16",
                "IPv6Address": ""
            },
            "870a1a4e86cb639ffe2bc143c06ed12810cd695ede9b1db0eca8ba94e73c5b88": {
                "Name": "springbootadmin",
                "EndpointID": "8e370f6df2ad761c4a92b6b2909a9fdfef87275d198a08b55e611d853c2f997d",
                "MacAddress": "02:42:ac:14:00:02",
                "IPv4Address": "172.20.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
```

4. Access the springboo admin console at http://localhost:9060 to confirm springboot microsevice is registed and status is "up". Use the credentials configured in springboot admin [application.properties](https://github.com/shivchikkappa/SpringbootServiceWithAdmin/blob/master/springbootadmin/src/main/resources/application.properties).

## Docker command options explained

-d                       --> Start the docker container in detach mode

--restart unless-stopped --> Start the container again without manual intevention if the host is restarted

--network <network name> --> Docker network the container needs to be associated
  
--name <container name>  --> Custom docker container name instead of radom name associated by docker 
  
-e <Key=value>           --> Springboot application properties override 

-p <port:port>           --> External port to be exposed and internal application port  

