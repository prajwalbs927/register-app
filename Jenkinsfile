pipeline{
  agent {label 'praju'}
  tools {
    jdk 'Java11'
    maven 'Maven3'
  }
  environment {
  APP_NAME = "my-pipeline"
  RELEASE = "1.0.0"
  DOCKER_USER = "prajwalbs927"
  DOCKER_PASS = 'dockerhub'
  IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
  IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
}
  stages {
    stage ('Clean up workspace' ) {
      steps {
      cleanWs()
      }
      }
stage ('Check out from SCM') {
steps {
  git branch: 'main', credentialsId: 'github', url: 'https://github.com/prajwalbs927/register-app.git'
    }
}  
stage ('Build Application') {
  steps {
    sh "mvn clean package"
  }
}
    stage ('Test Application') {
      steps {
        sh "mvn test"
      }
    }
    stage ('Sonarqube Analyis') {
    steps {
      script {
        withSonarQubeEnv(credentialsId: 'jenkins') {
        sh "mvn sonar:sonar"
        }
      }
    }
    }
    
  stage('Build and Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://hub.docker.com/repository/docker/prajwalbs927/praj9images/general', DOCKER_PASS) {
                        def docker_image = docker.build "${IMAGE_NAME}:${IMAGE_TAG}"
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }
  }
    
  } 
}
