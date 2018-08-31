pipeline {



    agent none


    stages {
        stage("Build-${pom.version}") {
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
        stage("Test-$JOB_NAME") {
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

        stage('Deploy-$JOB_NAME') {
            agent any
            steps {
            script {
                pom = readMavenPom file: 'pom.xml'
            }
            sh "echo ${pom.version}"
            sh "echo $JOB_NAME  "
            ansiblePlaybook (become: true, installation: 'tomcat deploy', extras: " -e Version=${pom.version}", playbook: 'playbook.yml')
            }
        }
    }
}
