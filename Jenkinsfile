pipeline {
    agent any 

    environment {
        DOCKER_IMAGE = "orvencasido/practice-2"
    }

    stages {
        stage('Build') {
            steps {
                script {
                    sh """
                        docker build -t ${DOCKER_IMAGE} .
                    """
                }
            }
        }

        stage('Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh """
                            echo $DOCKER_PASSWORD | docker login -u $DOCKER_USER --password-stdin
                            docker push ${DOCKER_IMAGE}
                        """
                }
            }
        }

        stage('Deploy') {
            steps {
                sh """
                    docker rm -f devops-project || true 
                    docker run -d -p 80:80 devops-project ${DOCKER_IMAGE
                    }
                """
            }
        }
    }
}