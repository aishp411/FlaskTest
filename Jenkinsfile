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

   stage('Deploy (staging)') {
  steps {
    sh '''
      ./venv/bin/gunicorn -b 0.0.0.0:5000 app:app --daemon
      sleep 5
      curl -f http://127.0.0.1:5000/ || (echo "Smoke test failed"; exit 1)
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
      if ! curl -f http://127.0.0.1:5000/; then
        echo "Smoke test failed"
        cat flask.log
        exit 1
      fi
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



