pipeline {
    agent any
    tools {
        maven "MAVEN3"
    }
  environment {
    JDK_VERSION = 'OracleJDK8'
    JAVA_HOME = '/usr/local/jdk1.8.0_131'
}

    stages {
        stage('fetch code') {
            steps {
                git branch: 'jenkins_sonar_nexus', url: "https://github.com/jkb91jkb91/vprofile_project/"
            }
        }

        stage('Build and Test') {
            steps {
                tool name: 'OracleJDK8', type: 'hudson.model.JDK'
                echo "${env.PRINT_OK}"
                sh 'mvn --version'
                sh 'mvn install -DskipTests'
            }
            post {
                success {
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    tool name: env.JDK_VERSION, type: 'hudson.model.JDK'
                    sh 'mvn test'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    tool name: 'OracleJDK11', type: 'hudson.model.JDK'
                    def scannerHome = tool name: 'son4.7', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    withSonarQubeEnv('sonar') {
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=project_vprofile \
                            -Dsonar.projectName=vprofile \
                            -Dsonar.projectVersion=1.0 \
                            -Dsonar.sources=src/ \
                            -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                            -Dsonar.junit.reportsPath=target/surefire-report/ \
                            -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                            -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml \
                            -Dsonar.java.source=11
                        """
                    }
                }
            }
        }

        stage('Code Analysis') {
            steps {
                script {
                    tool name: 'OracleJDK11', type: 'hudson.model.JDK'
                    nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: '172.17.0.3:8081/',
                        groupId: 'QA',
                        version: '1',
                        repository: 'vprofile-repo',
                        credentialsId: 'nexus',
                        artifacts: [
                            [artifactId: projectName,
                             classifier: '',
                             file: 'target/vprofile-v1.war',
                             type: 'jar']
                        ]
                    )
                }
            }
        }
    }
}
