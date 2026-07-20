pipeline {
    agent any 

    tools {
        maven 'maven'
        jdk 'jdk17'
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github-pat',
                    url: 'https://github.com/ReddyGV/devops_practice-'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean verify'
            }
        }

        stage('sonarQube Analysis'){
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
    }
}

