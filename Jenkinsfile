pipeline {




    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'

        }

    }
    stages {
        stage('Build-0.0.2') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test-0.0.2') {
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
            steps {
                ansiblePlaybook('playbook.yml') {
                 become(true)
                 becomeUser("root")
                 }
            }
        }

    }
}
