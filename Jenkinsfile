pipeline {
    agent any

    parameters {
        choice(
            name: 'MICROSERVICE',
            choices: ['booking-service'],
            description: 'Select which microservice to build and deploy'
        )
    }

    environment {
        DOCKER_IMAGE = "rdutta2/${params.MICROSERVICE}:${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: "https://github.com/RajKumarDutta/${params.MICROSERVICE}.git"
            }
        }

        stage('Build JAR') {
            steps {
                sh './mvnw clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE)
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials-id') {
                        docker.image(DOCKER_IMAGE).push()
                    }
                }
            }
        }
    }
}