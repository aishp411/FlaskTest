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
stage('Install Dependencies') {
  steps {
    sh 'sudo apt-get update && sudo apt-get install -y python3-venv python3-pip'
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
            echo '✅ Build and deployment successful!'
            mail to: 'aish.p411@gmail.com',
                 subject: "✅ Build Successful - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Great news! The build and deployment succeeded.\n\nSee details: ${env.BUILD_URL}",
                 from: 'noreply@yourproject.test'
        }
        failure {
            echo '❌ Build failed.'
            mail to: 'aish.p411@gmail.com',
                 subject: "❌ Build Failed - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Unfortunately, the build failed.\n\nCheck details: ${env.BUILD_URL}",
                 from: 'noreply@yourproject.test'
        }
    }
}



