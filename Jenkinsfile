
pipeline {
   agent any

    parameters {
        choice(
            name: 'ENVIRONMENT',
            choices: ['DEV', 'PROD'],
            description: 'Choose the deployment environment'
        )
    }

    tools {
        jdk 'jdk-21'
        maven 'maven-ci-server'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts(
                    artifacts: 'target/parameterized-when.war',
                    fingerprint: true
                )
            }
        }

        stage('Deploy DEV') {
            when {
                expression {
                    params.ENVIRONMENT == 'DEV'
                }
            }

            steps {
                deploy(
                    adapters: [
                        tomcat9(
                            credentialsId: 'jenkinsdeploy-tomcat',
                            url: 'http://54.87.201.69:8080'
                        )
                    ],
                    contextPath: '/myapp-dev',
                    war: 'target/parameterized-when.war'
                )
            }
        }

        stage('Deploy PROD') {
            when {
                expression {
                    params.ENVIRONMENT == 'PROD'
                }
            }

            steps {
                deploy(
                    adapters: [
                        tomcat9(
                            credentialsId: 'jenkinsdeploy-tomcat',
                            url: 'http://54.87.201.69:8080'
                        )
                    ],
                    contextPath: '/myapp-prod',
                    war: 'target/parameterized-when.war'
                )
            }
        }
    }
}

