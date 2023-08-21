def myFunctions = load './myFunctions.groovy'

pipeline {

    agent any
        tools {
        maven "MAVEN3"
        jdk "OracleJDK8"
    }
    environment {
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
                script {
                    echo "${env.PRINT_OK}"
                    myFunctions.printSomething()
                    myFunctions.printWithColor("colored text")
                    sh 'mvn --version'
                    sh 'mvn install -DskipTests'
                }
            }
            post {
                success {
             
                       
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

def printWithColor(param) {
     echo "\u001B[31m${param}\u001B[0m" 
}
