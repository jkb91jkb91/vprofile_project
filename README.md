

Docker Images
-Jenkins (PLUGINS > Sonar scanner
-Sonarqube (generate token called "jenkins")
-Nexus


PROBLEM
PROBLEMS CAN OCCUR ON JENKINS 
+ mvn install -DskipTests
[INFO] Scanning for projects...
[INFO] 
[INFO] ---------------------< com.visualpathit:vprofile >----------------------
[INFO] Building Visualpathit VProfile Webapp v1
[INFO] --------------------------------[ war ]---------------------------------
library initialization failed - unable to allocate file descriptor table - out of memoryAborted (core dumped)

URL solution >> https://superuser.com/questions/1413352/running-jdk-8-in-docker-suddenly-broken-on-arch-linux-with-unable-to-allocate-f




STEP 1 IF WE CREATE DOCKER IMAGE
JENKINS
Dockerfile >> Dockerfile.jenkins >> 
mkdir jenkins_config

#docker run -d -p 8080:8080 -p 50000:50000 -v $(pwd)/jenkins_config:/var/jenkins_home jenkins:lts
USE THIS COMMAND >> because of the "PROBLEM"
docker run -d -p 8080:8080 -p 50000:50000 --ulimit nofile=122880:122880 -m 3G -v $(pwd)/jenkins_config:/var/jenkins_home jen

cat /var/jenkins_home/secrets/initialAdminPassword
c1fdf0812a7e418e91bfbd05c907bd7b
u:jkb91
p:bjlm91
docker commit CONTAINERID jenkins_image_commited
docker run -d -p 8080:8080 -p 50000:50000 -v $(pwd)/jenkins_config:/var/jenkins_home jenkins_image_commited



SONAR
mkdir sonar_config/data 
mkdir sonar_config/conf
docker run -d -p 9000:9000 -p 9092:9092 -v $(pwd)/sonar_config/conf:/opt/sonarqube/conf -v $(pwd)/sonar_config/data:/opt/sonarqube/data sonar_image
u:admin
p:bjlm91
docker commit CONTAINERID sonar_image_commited



NEXUS
docker run -d -p 8081:8081 --name nexus -v nexus-data:/nexus-data sonatype/nexus3
u:admin
p:bjlm91



