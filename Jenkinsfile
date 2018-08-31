pipeline {



    agent none


    stages {
        stage('Build-0.0.2') {
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
            ansiblePlaybook (become: true, installation: 'tomcat deploy', extras: 'Version = "0.0.2"', playbook: 'playbook.yml')

            }
        }

    }
}
