pipeline {



    agent none


    stages {

	stage('checkout-0.0.3') {
	    agent any
            steps {
		checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions:
		[], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/chn555/spring-boot-examples']]])
            }
	}

        stage('Build-0.0.3') {
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
        stage('Test-0.0.3') {
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

        stage('Deploy-0.0.3') {
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
