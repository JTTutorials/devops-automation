pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven 'Maven 3.9.8'
    }

    stages {
        stage('Build Maven') {
            steps {
                // Get some code from a GitHub repository
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/JTTutorials/devops-automation']])

                // Run Maven on a Unix agent.
                bat "mvn clean install"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
        stage('Build Docker Image') {
            steps {
                script{
                    bat 'docker build -t arsule1985/devops-automation .'
                }
            }
       }
        stage('Push Docker Image to Hub') {
            steps {
                script{
                    withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                        bat 'docker login -u arsule1985 -p %dockerhubpwd%'
                    }
                    bat 'docker push arsule1985/devops-automation'
                }
            }
       }
    }
}
