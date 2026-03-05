pipeline {
    agent {
        label 'maven-agent'
    }
    options {
        skipStagesAfterUnstable()
    }
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKERHUB_USER',
                    passwordVariable: 'DOCKERHUB_PASS'
                )]) {
                    sh './jenkins/scripts/deliver.sh'
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}