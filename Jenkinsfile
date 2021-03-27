pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh '''gradle build
'''
        sh 'gradle javadoc'
        archiveArtifacts 'build/libs/*.jar'
      }
    }

  }
}