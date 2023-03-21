pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }
    tools {
        // specify the Docker installation
        dockerTool 'Docker'
    }
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    }
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t techsivam16/my-app .'
            }
        }
      /*   stage('Test') {
            steps {
                sh 'docker run --rm my-app go test'
            }
        } */
         /*
        stage('Login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('Push') {
            steps {
                sh 'docker push techsivam16/my-app'
            }
        }
        */
        stage('Push to Docker Hub') {
            steps {
                echo "Logging into Docker..."
                script{
                    try{
 withCredentials([usernamePassword(credentialsId: 'DOCKER_HUB_CREDENTIALS_ID', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh " echo \${DOCKER_PASSWORD} | docker login --username \${DOCKER_USERNAME} --password-stdin &&\
                            docker tag my-app \${DOCKER_USERNAME}/my-app  &&\
                            docker push \${DOCKER_USERNAME}/my-app"
 }
                    }catch(err){
                        error("Failed: $(err)")
                    }
                }

               /*  withCredentials([usernamePassword(credentialsId: 'DOCKER_HUB_CREDENTIALS_ID', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    // sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                    sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"

                    sh 'docker tag my-app $DOCKER_USERNAME/my-app'
                    sh 'docker push $DOCKER_USERNAME/my-app'
                } */
            }
        }
          /*
         stage('Deploy to Kubernetes') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'KUBERNETES_CREDENTIALS_ID', usernameVariable: 'KUBERNETES_USERNAME', passwordVariable: 'KUBERNETES_PASSWORD')]) {
                    sh 'kubectl config set-credentials my-user --username=$KUBERNETES_USERNAME --password=$KUBERNETES_PASSWORD'
                    sh 'kubectl config set-context my-context --cluster=my-cluster --user=my-user'
                    sh 'kubectl config use-context my-context'
                    sh 'kubectl apply -f kubernetes/deployment.yaml'
                    sh 'kubectl apply -f kubernetes/service.yaml'
                }
            }
        } */
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}
