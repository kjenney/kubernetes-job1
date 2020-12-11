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
    stage('Run maven') {
      steps {
        container('maven') {
          sh 'mvn -version'
          def emails = readYaml file: 'test.yml'
          echo "${emails}"
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
