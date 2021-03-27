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

    stage('Code Analysis') {
      parallel {
        stage('Code Analysis') {
          steps {
            bat 'gradle sonarqube'
            waitForQualityGate true
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
        slackSend(token: 'T01LX7C6ZBR/B01ST4JG18C/E4T83lk6QKPWfi0wnZWgrRWK', baseUrl: 'https://hooks.slack.com/services/', channel: '#project', message: 'Project is built newly and deployed')
      }
    }

  }
}