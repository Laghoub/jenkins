pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        bat 'gradle generateMatrixAPI'
        archiveArtifacts 'build/libs/*.jar'
      }
    }
    stage('Mail Notification') {
      steps {
        mail(subject: 'Jenkins notification', body: 'une nouvelle push dans Github', cc: 'hb_zatout@esi.dz')
      }
    }

  }
}