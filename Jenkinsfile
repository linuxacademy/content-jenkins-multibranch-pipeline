pipeline {
  agent any

  stages {
    stage('build') {
      steps {
        sh 'cat /etc/hostname'
        sh 'echo résultat du test > test.log'
      }
    }
    stage('run') {
      steps {
        sh 'cat /etc/hostname'
      }
    }
  }
  post {
    success {
      archiveArtifacts artifacts: 'test.log', fingerprint: true
    }
  }
}
