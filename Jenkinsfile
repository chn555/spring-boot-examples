pipeline {


    environment {
         VERSION = "${env.POM_VERSION}"
    }

    agent none


    stages {
        stage('Build-${env.POM_VERSION}') {
            agent {
              docker {
                  image 'maven:3'
                  args '-v /root/.m2:/root/.m2'
               }
            }
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test-0.0.2') {
            agent {
              docker {
                  image 'maven:3'
                  args '-v /root/.m2:/root/.m2'
               }
            }
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deploy-0.0.2') {
            agent any
            steps {
            sh 'echo ${env.POM_VERSION}'
            sh 'echo $POM_VERSION'
            sh 'ehco ${POM_VERSION}'
            
            ansiblePlaybook (become: true, installation: 'tomcat deploy', playbook: 'playbook.yml')

            }
        }

    }
}
