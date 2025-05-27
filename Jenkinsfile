@Library('Shared') _

pipeline{
    agent any

        environment {
            IMAGE_NAME = "notes-app"
            IMAGE_TAG = "latest"
        }

    stages{
        stage('Hello'){
            steps{
                script{
                    hello()
                }
            }
        }
        stage("Code"){
            steps{
                echo "This is cloning the code"
                git url: "https://github.com/DeVeNdRa1910/django-notes.git", branch: "main"
                echo "Cloning of the code is successfull"
            }
        }
        stage("Docker Login") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerHubCred', usernameVariable: 'DOCKER_HUB_USER', passwordVariable: 'DOCKER_HUB_PASS')]) {
                    echo "Logging in to Docker Hub"
                    sh 'echo "$DOCKER_HUB_PASS" | docker login -u "$DOCKER_HUB_USER" --password-stdin'
                }
            }
        }
        stage("Build"){
            steps{
                echo "This is building the code"
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }
        stage("Push to Docker Hub"){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerHubCred', usernameVariable: 'DOCKER_HUB_USER', passwordVariable: 'DOCKER_HUB_PASS')]){
                    echo "This stage is pushing Docker image on DockerHub" 
                    sh '''
                        docker image tag ${IMAGE_NAME}:${IMAGE_TAG} ${DOCKER_HUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}
                        docker image push ${DOCKER_HUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}  
                    '''
                }
            }
        }
        stage("Test"){
            steps{
                echo "This is testing the code"
            }
        }
        stage("Deploy"){
            steps{
                echo "This is Deploying the code"
                sh "docker compose down || true"
                sh "docker compose up -d"
            }
        }
    }
}