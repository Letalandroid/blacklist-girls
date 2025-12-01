pipeline {
  agent { label 'raspi-agent' } // usa el label del nodo (ajusta si lo llamaste distinto)
  stages {
    stage('Checkout') {
      steps {
        checkout scm
        sh 'pwd; ls -la'
      }
    }
    stage('Install') {
      steps {
        // usa npm ci si tienes package-lock.json, npm install si no
        sh '''
          if [ -f package.json ]; then
            npm ci || npm install
          else
            echo "No package.json: skipping npm install"
          fi
        '''
      }
    }
    stage('Build / Serve') {
      steps {
        sh '''
          if [ -f package.json ]; then
            npm run build || true
          fi
          # intenta usar npx serve (no requiere instalación global)
          if command -v npx >/dev/null 2>&1 ; then
            npx --yes serve -s . -l 4000 &
          else
            echo "npx no disponible; asegúrate de tener node/npm en el agente"
          fi
        '''
      }
    }
  }
  post {
    always {
      archiveArtifacts artifacts: '**/dist/**', allowEmptyArchive: true
    }
  }
}
