pipeline {
  agent any

  environment {
    DOCKER_IMAGE = "rdutta2/${params.MICROSERVICE}:${BUILD_NUMBER}"
  }

  stages {
    stage('Init Parameters') {
      steps {
        script {
          properties([
            parameters([
              choice(
                name: 'MICROSERVICE',
                choices: ['booking-service', 'hotel-service', 'payment-service'],
                description: 'Select which microservice to build'
              )
            ])
          ])
        }
      }
    }
    stage('Checkout') {
      steps {
        git branch: 'main',
          credentialsId: 'github_secrets',
          url: 'https://github.com/RajKumarDutta/nu_microservices.git'
      }
    }

    stage('Build JAR') {
      steps {
        dir("${params.MICROSERVICE}") {
          sh './mvnw clean package -DskipTests'
        }
      }
    }

    stage('Docker Build') {
      steps {
        script {
          docker.build(DOCKER_IMAGE, "${params.MICROSERVICE}")
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