pipeline {

    
    agent any
        tools {
        maven "MAVEN3"
        jdk "OracleJDK8"
    }
    enivironment {
        PRINT_OK='ok'
        PRINT_FAIL='fail'
    }
    

    stages{
        stage('fetch code') {
          steps{
              git branch: 'jenkins_sonar_nexus', url: "https://github.com/jkb91jkb91/vprofile_project/"
          }  
        }

        stage('Build') {
            steps {
                echo ${env.PRINT_OK}
                printSomething() //trigger groovy func
                sh 'mvn --version'
                sh 'mvn install -DskipTests'
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

def printSomething() {
    echo "groovy func"
}
