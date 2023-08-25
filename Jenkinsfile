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
        stage('Fetch Code') {
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
                 script {
                    def scannerHome = tool name: 'son4.7', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    withSonarQubeEnv('sonar') {
                         sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=project_vprofile \
                            -Dsonar.projectName=vprofile \
                            -Dsonar.sources=src/ \
                            -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml
                        """
                    }
                }
            }
        }
    }
}
