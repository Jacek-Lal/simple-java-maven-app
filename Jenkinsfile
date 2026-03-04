pipeline {
    agent any
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
                    echo 'Tested'

                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
            post {
                always {
                    echo 'Delivered'
                }
            }
        }
    }
}