String message = "";
pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        bat 'gradle generateMatrixAPI'
        archiveArtifacts 'build/libs/*.jar'
      }
      post{
        failure{
            message = "build failed"
        }
        success{
            message = "build successful"
        }
      }
    }

    stage('Mail Notification') {
      steps {
        mail(subject: 'Jenkins notification', body: message, cc: 'hb_zatout@esi.dz')
      }
    }

  }
}