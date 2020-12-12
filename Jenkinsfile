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
                //def imageTag = 'feature/someBranchName'
                //def command = $/echo ${imageTag} | sed 's/\//\-/'/$
                //imageTag = sh(returnStdout: true, script: command).trim()
                //sh("echo ${imageTag}")
                def map_script= $/grep ${BUILD_USER_EMAIL} test.csv | awk -F ',' '{print $2}'/$
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
