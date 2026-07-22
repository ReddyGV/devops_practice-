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
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Run Application') {
            steps {
                sh 'java -cp target/devops-practice-1.0.0.jar com.devops.App'
            }
        }

        stage('upload to JFrog') {
            steps  {
                withCredentials([string(credentialsId: 'jfrog-token', variable: 'TOKEN')])
                    sh '''
                    curl -H "Authorization: bearer $TOKEN" \
                    -T target/devops-practice-1.0.0.jar \
                    http://artifactory:8082//http://localhost:8082/ui/admin/repositories/local/maven-local/edit/com/devops/devops-practice/1.0.0/devops-practice-1.0.0.jar
                    '''
            }
        }
    }
}