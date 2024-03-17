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
     stage ('Quality Gate') {
      steps {
        script {
          waitForQualityGate abortPipeline: false, credentialsId: 'jenkins'
        }
      }
    }
   stage ('Build and Push docker Image') {
     steps {
       script {
         docker.withRegistry(' ',DOCKER_PASS) {
           docker_image = docker.build "${IMAGE_NAME}"
         }
         docker.withRegistry(' ',DOCKER_PASS) {
           docker_image.push("${IMAGE_TAG}")
           docker_image.push('latest')
         }
       }
     }
   }
    
  } 
}
