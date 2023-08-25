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

    stages {
        stage('fetch code') {
            steps {
                git branch: 'jenkins_sonar_nexus', url: "https://github.com/jkb91jkb91/vprofile_project/"
            }
        }

        stage('Build') {
            steps {
                script {
                    echo "${env.PRINT_OK}"
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

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Code Analysis') {
            steps {
               nexusArtifactUploader(
                   nexusVersion: 'nexus3',
                   protocol: 'http',
                   nexusUrl: 'http://172.17.0.3:8081/',
                   groupId: 'QA',
                   version: '1',
                   repository: 'vprofile-repo',
                   credentialsId: 'nexus'
                   artifacts: [
                    [artifactId: projectName,
                    classifier: '',
                    file: 'my-service-' + version + 'jar',
                    typye: 'jar']
                       ]

                   }
        }
    }
}
