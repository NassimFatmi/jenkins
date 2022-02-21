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
      steps {
        sh 'gradle sonarqube'
      }
    }

  }
}