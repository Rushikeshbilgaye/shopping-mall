pipeline {

```
agent any

environment {
    IMAGE_NAME = "YOUR_DOCKERHUB_USERNAME/shopping-mall"
    CONTAINER_NAME = "shopping-mall-container"
    PORT = "80"
}

stages {

    stage('Checkout') {
        steps {
            echo 'Cloning source code from GitHub...'

            git branch: 'main',
                url: 'https://github.com/YOUR_USERNAME/shopping-mall.git'
        }
    }

    stage('Build Docker Image') {
        steps {
            echo 'Building Docker image...'

            sh '''
                docker build -t ${IMAGE_NAME}:latest .
            '''
        }
    }

    stage('Docker Login') {
        steps {
            echo 'Logging into Docker Hub...'

            withCredentials([
                usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USERNAME',
                    passwordVariable: 'DOCKER_PASSWORD'
                )
            ]) {

                sh '''
                    echo "$DOCKER_PASSWORD" | docker login \
                    -u "$DOCKER_USERNAME" \
                    --password-stdin
                '''
            }
        }
    }

    stage('Push Image to Docker Hub') {
        steps {
            echo 'Pushing image to Docker Hub...'

            sh '''
                docker push ${IMAGE_NAME}:latest
            '''
        }
    }

    stage('Stop Existing Container') {
        steps {
            echo 'Stopping old container...'

            sh '''
                docker stop ${CONTAINER_NAME} || true
            '''
        }
    }

    stage('Remove Existing Container') {
        steps {
            echo 'Removing old container...'

            sh '''
                docker rm ${CONTAINER_NAME} || true
            '''
        }
    }

    stage('Deploy Container') {
        steps {
            echo 'Starting new container...'

            sh '''
                docker run -d \
                --name ${CONTAINER_NAME} \
                -p ${PORT}:80 \
                ${IMAGE_NAME}:latest
            '''
        }
    }
}

post {

    success {
        echo '✅ UrbanMall deployed successfully!'
    }

    failure {
        echo '❌ Deployment failed. Check Jenkins console output.'
    }

    always {
        sh 'docker ps'
    }
}
```

}
