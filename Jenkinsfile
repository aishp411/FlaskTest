pipeline {
    agent { label 'agent1' }

    environment {
        VENV_DIR = 'venv'
    }

    
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/aishp411/FlaskTest.git', branch: 'main'
            }
        }

        stage('Setup Virtual Environment') {
            steps {
                sh 'python3 -m venv $VENV_DIR'
                sh '. $VENV_DIR/bin/activate && pip install --upgrade pip'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '. $VENV_DIR/bin/activate && pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh '. $VENV_DIR/bin/activate && pytest --maxfail=1 --disable-warnings -q'
            }
        }

        stage('Deploy') {
  when { branch 'main' }   // only deploy from main (adjust as needed)
  steps {
    sh '''
      echo "Deploy: copying workspace to /opt/flaskapp"
      rsync -a --delete "$WORKSPACE/" /opt/flaskapp/

      echo "Create venv if missing"
      python3 -m venv /opt/flaskapp/venv || true

      echo "Install requirements"
      . /opt/flaskapp/venv/bin/activate
      pip install --upgrade pip
      pip install -r /opt/flaskapp/requirements.txt

      echo "Restart systemd service"
      sudo systemctl restart flaskapp
      sudo systemctl status flaskapp --no-pager
    '''
  }
}

    }

    post {
        always {
            sh 'deactivate || true'
        }
    }
}
