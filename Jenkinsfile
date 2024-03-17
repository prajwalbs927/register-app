pipeline{
  agent {label 'node2'}
  tools {
    jdk 'Java11'
    maven 'Maven3'
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
        agent {label 'built-in'}
        script {
          waitForQualityGate abortPipeline: false, credentialsId: 'jenkins'
        }
      }
    }
  
    
  } 
}
