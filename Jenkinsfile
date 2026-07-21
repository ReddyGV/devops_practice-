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

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh 'mvn sonar:sonar'
                }
            }

        stage('Run Application') {
            steps {
                sh 'java -cp target/devops-practice-1.0.0.jar com.devops.App'
            }
        }

        stage('Quality gate') {
            steps {
                timeout(time: 2, unit: 'MINITUS') {
                    waitForQualityGate abortPipeline: True
                }
            }
        }
    }
}