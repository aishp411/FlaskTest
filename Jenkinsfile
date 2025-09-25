pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
        DEPLOY_DIR = '/opt/flaskapp'
        CONTACT_EMAIL='ash.p411@gmail.com'
  
    }
 
    stages {
        stage('Checkout') {
            steps {
                // Checkout code from GitHub
                git url: 'https://github.com/aishp411/FlaskTest.git', branch: 'main'
            }
        }

        stage('Setup Virtual Environment') {
            steps {
                sh '''
                python3 -m venv $VENV_DIR
                . $VENV_DIR/bin/activate
                pip install --upgrade pip
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                . $VENV_DIR/bin/activate
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                . $VENV_DIR/bin/activate
                pytest --maxfail=1 --disable-warnings -q
                '''
            }
        }

    stage('Deploy') {
    steps {
        sh '''
        set -e
        # Copy project files to deploy directory
        rsync -av --delete . /home/jenkins/flaskapp/

        cd /home/jenkins/flaskapp

        # Create virtual environment if not exists
        if [ ! -d venv ]; then
            python3 -m venv venv
        fi

        # Upgrade pip and install dependencies
        venv/bin/pip install --upgrade pip
        venv/bin/pip install -r requirements.txt

        # Restart service (if you use systemd, make sure Jenkins user can do this)
        # sudo systemctl restart flaskapp  # remove sudo if not needed
        '''
    }
}

    }

   
  post {
    success {
      mail to: "${env.CONTACT_EMAIL}", subject: " build ${env.BUILD_NUMBER} SUCCESS", body: "${env.BUILD_URL}"
    }
    failure {
      mail to: "${env.CONTACT_EMAIL}", subject: " build ${env.BUILD_NUMBER} FAILED", body: "${env.BUILD_URL}"
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
