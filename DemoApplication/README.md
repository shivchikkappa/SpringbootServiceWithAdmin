# Technologies
JDK 11

Springboot 2.2.1

Maven 3.6.1

Docker

# Build and Run the application without docker

1. Clone the repo from github 

2. mvn clean install

3. java -jar target/DemoPractice-0.0.1-SNAPSHOT.jar

4. Application will start running on port 9080

# Build the application docker image

1. Clone the repo from github

2. mvn clean install
    
3. Build the image 

    docker build . -t mydemoapplication
    
4. Confirm image build is successful

    docker image ls
    
    mydemoapplication                latest              e4e20caa8cfb        About a minute ago   432MB    

5. Start the container 
    
    docker run -d --restart unless-stopped --name mydemoapplication -p 9080:9080 -p 9085:9085 mydemoapplication
    
# Run the application using docker image

1. Start the container using below command 
    
    docker run -d --restart unless-stopped --name mydemoapplication -p 9080:9080 -p 9085:9085 mydemoapplication
    
2. Give a moment for container to start and check if it's running using below command

    docker container ls
    
    213bf1d8cc92        mydemoapplication        "java -jar /demoprac…"   9 seconds ago       Up 8 seconds        0.0.0.0:9080->9080/tcp, 0.0.0.0:9085->9085/tcp   mydemoapplication

3. Stop and remove the container

    docker container stop mydemoapplication
    docker container rm mydemoapplication
