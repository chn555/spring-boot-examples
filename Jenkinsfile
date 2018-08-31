pipeline {

    agent any {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }

       environment {
          VERSION = readMavenPom().getVersion()
       }
    }
    stages {
        stage('Build-$VERSION') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test-$VERSION') {
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
