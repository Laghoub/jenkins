pipeline {
  agent any
  stages {
    stage('build') {
      post {
        failure {
          script {
            mail="Build failed"
          }

        }

        success {
          script {
            mail="Build succeeded"
          }

        }

      }
      steps {
        bat 'gradle build'
        bat 'gradle javadoc'
        archiveArtifacts 'build/libs/*.jar'
        junit(testResults: 'build/test-results/test/*.xml', allowEmptyResults: true)
      }
    }

    stage('Mail Notification') {
      steps {
        mail(subject: 'Jenkins notification', body: mail, cc: 'hb_zatout@esi.dz')
      }
    }

    stage('Code Analysis') {
      parallel {
        stage('Code Analysis') {
          when {
            branch 'master'
          }
          steps {
            withSonarQubeEnv('SonarQube Server') {
              bat(script: 'gradle sonarqube', returnStatus: true)
            }

            timeout(time: 10, unit: 'MINUTES') { 
              script {
                  sleep 120
                 def qg = waitForQualityGate() 
                 if (qg.status != 'OK') {
                   error "Pipeline aborted due to quality gate failure: ${qg.status}"
                 }
              }
            }
          }
        }

        stage('Test Reporting') {
          steps {
            cucumber 'reports/*json'
          }
        }

      }
    }

    stage('Deployment') {
      steps {
        bat 'gradle publish'
      }
    }

    stage('Slack Notification') {
      steps {
        slackSend(token: 'T01LX7C6ZBR/B01SJQJ7LF7/76DVeKq505GAktJwLyewt5u6', baseUrl: 'https://hooks.slack.com/services/', channel: '#project', message: 'Project is built newly and deployed')
      }
    }

  }
  environment {
    mail = ''
  }
}