pipeline {
  agent any

  tools {
    maven 'localMaven'
  }

  stages{
    stage('Build'){
      steps {
        bat 'mvn clean package'
      }
      post {
        success {
          echo 'Now Archiving...'
          archiveArtifacts artifacts: '**/target/*.war'
        }
      }
    }
    stage('Deploy to Staging'){
      steps {
        build job: 'deploy-to-staging-pipeline'
      }
    }
    stage('Deploy to Production'){
      steps {
        timeout(time:5, until:'DAYS'){
          input message:'Approved PRODUCTION Deployment?'
        }

        build job: 'deploy-to-prod-pipeline'
      }
      post {
        success {
          echo 'Code deployed to production.'
        }

        failure {
          echo 'Deployment failed.'
        }
      }
    }
  }
}
