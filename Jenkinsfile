pipeline {
    agent any

    stages{
        stage('fetch code') {
          steps{
              git branch: 'jenkins_sonar_nexus', url: "https://github.com/jkb91jkb91/vprofile_project/blob/jenkins_sonar_nexus/Jenkinsfile"
          }  
        }

        stage('Build') {
            steps {
                sh 'mvn clean install -DskipTests'
            }
            post {
                success {
                    echo "Now Archiving."
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage('Test'){
            steps {
                sh 'mvn test'
            }

        }
         stage('Code Ananlysis'){
            steps {
                sh 'mvn checkstyle:checkstyle'
            }

        }
    }
}
