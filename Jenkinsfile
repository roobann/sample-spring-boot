pipeline {
    agent any

    tools {
        maven 'Maven' // The Maven installation name configured in Jenkins
    }

    environment {
        SERVER_IP = '192.168.1.110'
        DEPLOY_USER = 'rooban'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/roobann/sample-spring-boot.git'  // Your Git repo URL
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    def jarFile = 'target/sample-spring-1.0.jar'
                    sh """
                        scp -o StrictHostKeyChecking=no ${jarFile} ${DEPLOY_USER}@${SERVER_IP}:/home/${DEPLOY_USER}/app/
                        ssh -o StrictHostKeyChecking=no ${DEPLOY_USER}@${SERVER_IP} 'pkill -f java || true'
                        ssh -o StrictHostKeyChecking=no ${DEPLOY_USER}@${SERVER_IP} 'nohup java -jar /home/${DEPLOY_USER}/app/sample-spring-1.0.jar &'
                    """
                }
            }
        }
    }

    /* post {
        always {
            junit '**//* target/surefire-reports *//*.xml'
        }
    } */
}
