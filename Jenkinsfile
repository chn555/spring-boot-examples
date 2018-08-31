pipeline {

    agent any

    environment {
       VERSION = readMavenPom().getVersion()
    }


    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
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
