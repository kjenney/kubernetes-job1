@Library('shared-library') _

pipeline {
  agent {
    kubernetes {
      label 'promo-app'
      idleMinutes 5
      yamlFile 'build-pod.yaml'
      defaultContainer 'maven'
    }
  }
  stages {
    stage('Run stuff') {
      steps {
        wrap([$class: 'BuildUser']) {
          container('maven') {
            sh 'mvn -version'
            script {
              def USER_ID = csv.matchValue(${BUILD_USER_EMAIL},'email','userid')
              if ( USER_ID ) {
                echo "${BUILD_USER_EMAIL} matched to ${USER_ID}"
              } else {
                error("Failed to find user id for ${BUILD_USER_EMAIL}")
              }
            }
          }
        container('busybox') {
          sh '/bin/busybox'
        }
      }
    }
  }
  post {
    always {
      sleep 10
      deleteDir()
    }
  }
}
