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
                echo "The File exists :)"
                def map_script= $/grep ^${BUILD_USER_EMAIL} test.csv | awk -F ',' '{print $2}'/$
                USER_ID = sh(returnStdout: true, script: map_script).trim()
                echo "${USER_ID}"
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
