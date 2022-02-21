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
        mail(subject: 'Mail Notification', body: 'Build Done', cc: 'nassim.gti25@gmail.com', from: 'in_fatmi@esi.dz')
      }
    }

    stage('Code Analysis') {
      parallel {
        stage('Code Analysis') {
          post {
            success {
              mail(subject: 'Code Analysis Succussful', body: 'Fin d\'analyse avec success', cc: 'nassim.gti25@gmail.com', from: 'in_fatmi@esi.dz')
            }

            failure {
              mail(subject: 'Code analysis Failure', body: 'Fin avec failure', cc: 'nassim.gti25@gmail.com', from: 'in_fatmi@esi.dz')
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

  }
}