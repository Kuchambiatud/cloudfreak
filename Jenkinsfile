pipeline {
    agent any

    tools {
        maven 'maven' // Check that 'maven' matches the Maven installation in Jenkins Global Tool Configuration
        jdk 'JDK8'    // Check that 'JDK8' matches the JDK installation in Jenkins Global Tool Configuration
    }

    stages {
        stage('Build') {
            steps {
                sh 'pwd'
                sh 'mvn clean install package'
            }
        }

        stage('Copy Artifact') {
            steps {
                sh 'pwd'
                sh 'cp -r target/*.jar docker'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def customImage = docker.build('initsixcloud/petclinic', "./docker")
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        customImage.push("${env.BUILD_NUMBER}")
                    }
                }
            }
        }
    }
}

