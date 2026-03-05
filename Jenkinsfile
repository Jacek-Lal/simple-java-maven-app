pipeline {
    agent {
        label 'maven-agent'
    }
    options {
        skipStagesAfterUnstable()
    }
    triggers {
        pollSCM('H/5 * * * *')
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
        stage('Build Docker Image') {
            steps {
                sh './jenkins/scripts/build_image.sh'
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