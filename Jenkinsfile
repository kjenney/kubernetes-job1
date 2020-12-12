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
              Boolean bool = fileExists 'test.csv'
              if (bool) {
                println "The File exists :)"
                USER_ID = sh(script: "/bin/bash -c 'grep ${BUILD_USER_EMAIL} test.csv | awk -F \',\' \'{print \$2}\''", returnStdout: true)
                println "${USER_ID}"
              }
              else {
                println "The File does not exist :("
              }
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
