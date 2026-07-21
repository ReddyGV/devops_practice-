pipeline {
    agent any 

    tools {
        maven 'maven'
        jdk 'jdk'
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
        
        stage('Run application'){
            steps {
                sh 'java -cp target/devops-practice-1.0.0.jar com.devops.App'
                }
            }
        }
    }
}

