pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh '/usr/bin/docker build -t devsecops:latest .'
      }
    }
    stage('Scan') {
      steps {
        sh '/usr/local/bin/trivy  --exit-code 1 --severity HIGH,CRITICAL repository https://github.com/paredes-jr/devsecops-trivy.git'
      }
    }
    stage('Approval') {
      steps {
        input(message: 'Approval required before Terraform', ok: 'Proceed', submitterParameter: 'APPROVER')
      }
    }
    stage('Deploy') {
      steps {
                sh '/usr/bin/docker run -d -p 5050:80 devsecops:latest'
      }
    }
  }
}
