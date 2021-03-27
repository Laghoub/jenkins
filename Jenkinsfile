pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        bat 'gradle generateMatrixAPI'
        archiveArtifacts 'build/libs/*.jar'
      }
    }

  }
}