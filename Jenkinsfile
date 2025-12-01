pipeline {
  agent { label 'raspi' }
  stages {
    stage('Test host tools') {
      steps {
        sh 'whoami; pwd; node -v || true; npm -v || true; python3 --version || true'
      }
    }
  }
}
