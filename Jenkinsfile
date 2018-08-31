pipeline {


    environment {
         VERSION = "${env.POM_VERSION}"
         MAVEN_VERSION=`grep A -2 -B 2 "<your_project_name>" pom.xml | grep version | cut -d\> -f 2 | cut -d\< -f 1`-commit-"`echo $GIT_COMMIT`"

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
            sh 'echo ${POM_VERSION}'
            sh 'echo ${MAVEN_VERSION}'
            ansiblePlaybook (become: true, installation: 'tomcat deploy', playbook: 'playbook.yml')

            }
        }

    }
}
