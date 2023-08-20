

Docker Images
-Jenkins
-Sonarqube
-Nexus



STEP 1 IF WE CREATE DOCKER IMAGE
JENKINS
mkdir jenkins_config
docker run -d -p 8080:8080 -p 50000:50000 -v $(pwd)/jenkins_config:/var/jenkins_home jenkins:lts
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





