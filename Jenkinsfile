pipeline {




    agent any

    
    environment {
        PATH = "/usr/bin/ansible:/usr/bin/ansible-playbook:$PATH"
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
            sh 'which ansible'
            ansiblePlaybook become: true, installation: 'tomcat deploy', playbook: 'playbook.yml'

            }
        }

    }
}
