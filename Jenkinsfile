pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh 'gradle build'
        sh 'gradle javadoc'
        archiveArtifacts 'build/libs/*.jar'
        archiveArtifacts 'build/docs/**'
        archiveArtifacts 'build/reports/**'
      }
    }

    stage('Mail Notification') {
      steps {
        mail(subject: 'TP Jenkins Fin de build', body: 'Build Done', cc: 'in_fatmi@esi.dz', from: 'in_fatmi@esi.dz')
        mail(subject: 'TP Jenkins Fin de build', body: 'Build done', cc: 'ks_cherfaoui@esi.dz ', from: 'in_fatmi@esi.dz')
      }
    }

    stage('Code Analysis') {
      parallel {
        stage('Code Analysis') {
          post {
            success {
              mail(subject: 'Analyse de Code', body: 'Fin de l\'analyse de code avec succes', cc: 'in_fatmi@esi.dz', from: 'in_fatmi@esi.dz')
              mail(subject: 'Analyse de Code', body: 'Fin de l\'analyse de code avec succes', cc: 'ks_cherfaoui@esi.dz ', from: 'in_fatmi@esi.dz')
            }

            failure {
              mail(subject: 'Analyse de Code', body: 'Fin de l\'analyse de code avec un statu d\'echec', cc: 'in_fatmi@esi.dz', from: 'in_fatmi@esi.dz')
              mail(subject: 'Analyse de Code', body: 'Fin de l\'analyse de code avec un statu d\' echec', cc: 'ks_cherfaoui@esi.dz ', from: 'in_fatmi@esi.dz')
            }

          }
          steps {
            sh 'gradle sonarqube'
          }
        }

        stage('Test Reporting') {
          steps {
            sh 'gradle cucumberCli'
          }
        }

      }
    }

    stage('Deployment') {
      steps {
        sh 'gradle publish'
      }
    }

    stage('Slack Notification') {
      steps {
        slackSend(baseUrl: 'https://hooks.slack.com/services/', token: 'T0346VD89RQ/B0358STH68Y/Bz9fx3n1wghrYI9CB8Fvk9Ft', channel: 'tp_jenkins', message: 'From jenkins pipelone')
      }
    }

  }
}
