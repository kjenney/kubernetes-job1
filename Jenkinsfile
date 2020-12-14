@Library('upstream-library-implicit') _
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
              def match = csv.matchValue('apples','type','origin')
              if ( match ) { println "${match}" }
              //def map_script= $/grep \^${BUILD_USER_EMAIL} test.csv | awk -F ',' '{print $2}'/$
              //USER_ID = sh(returnStdout: true, script: map_script).trim()
              //echo "${USER_ID}"
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
