pipeline {




    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'

        }

    }
    stages {
        stage('Build-${env.BUILD_ID}) {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test-${env.BUILD_ID}') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    }
}