pipeline {
  agent {
    kubernetes {
      label 'promo-app'
      idleMinutes 5
      yamlFile 'build-pod.yaml'
      defaultContainer 'maven'
    }
  }
  parameters {
    string(name: 'email', defaultValue: 'test@email.com', description: 'Which email should I look for?')
  }
  stages {
    stage('Run maven') {
      steps {
        container('maven') {
          sh 'mvn -version'
          script {
            Boolean bool = fileExists 'test.yml'
            if (bool) {
              println "The File exists :)"
              def emails = readYaml file: 'test.yml'
              assert emails['emails'][0]['email'] == "${email}"
              println "It's there!"
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
