pipeline {
  agent {
    kubernetes {
      label 'promo-app'
      idleMinutes 5
      yamlFile 'build-pod.yaml'
      defaultContainer 'maven'
    }
  }
  node {
    wrap([$class: 'BuildUser']) {
      def user = env.BUILD_USER_ID
    }
    stages {
      stage('Run maven') {
        steps {
          container('maven') {
            sh 'mvn -version'
            script {
              Boolean bool = fileExists 'test.csv'
              if (bool) {
                println "The File exists :)"
                id = sh(returnStdout: true, script: "grep ^\$user test.csv | awk -F ',' '{print \$2}'")
                println "${id}"
              }
              else {
                println "The File does not exist :("
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
}
