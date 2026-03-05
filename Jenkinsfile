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
            post {
                always {
                    echo 'Built'
                }
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