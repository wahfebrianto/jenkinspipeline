pipeline{
  agent any

  tools {
    maven 'localMaven'
  }

  parameters {
    string(name: 'tomcat_dev', defaultValue: '18.224.16.34', description: 'Staging Server')
    string(name: 'tomcat_prod', defaultValue: '18.216.131.55', description: 'Production Server')
  }

  triggers {
    pollSCM('* * * * *')
  }

  stages {
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

    stage('Deployment'){
      parallel{
        stage('Deploy to Staging'){
          steps{
            bat "pscp -i E:/Kerjaan/_Jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
          }
        }

        stage('Deploy to Production'){
          steps{
            bat "pscp -i E:/Kerjaan/_Jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
          }
        }
      }
    }
  }
}
