pipeline {
  agent any

  environment {
    APP_NAME = 'FlaskTest'
    CONTACT_EMAIL = 'aish.p411@gmail.com'
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/aishp411/FlaskTest.git'
      }
    }

    stage('Build') {
      steps {
        sh '''
          python3 -m venv venv
          ./venv/bin/pip install --upgrade pip
          ./venv/bin/pip install -r requirements.txt
        '''
      }
    }

    stage('Test') {
      steps {
        sh './venv/bin/pytest -q --maxfail=1'
      }
    }

    stage('Deploy (staging)') {
      steps {
        sh '''
          nohup ./venv/bin/python3 app.py > flask.log 2>&1 &
          echo $! > flask.pid
          sleep 5
          curl -f http://127.0.0.1:5000/ || (echo "Smoke test failed"; cat flask.log; exit 1)
        '''
      }
    }
  }

  post {
    success {
      // requires mailer plugin, if not available use a custom script
      mail to: "${env.CONTACT_EMAIL}",
           subject: "${env.APP_NAME} build #${env.BUILD_NUMBER} SUCCESS",
           body: "See details: ${env.BUILD_URL}"
    }
    failure {
      mail to: "${env.CONTACT_EMAIL}",
           subject: "${env.APP_NAME} build #${env.BUILD_NUMBER} FAILED",
           body: "See details: ${env.BUILD_URL}"

      sh '''
        if [ -f flask.pid ]; then
          kill $(cat flask.pid) || true
          rm -f flask.pid
        fi
      '''
    }
    always {
      archiveArtifacts artifacts: 'flask.log', allowEmptyArchive: true
    }
  }
}
