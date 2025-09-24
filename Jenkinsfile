pipeline {
    agent { label 'agent1' }

    environment {
        VENV_DIR = 'venv'
        DEPLOY_DIR = '/opt/flaskapp'
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

        
    }

    post {
        always {
            sh 'deactivate || true'
        }
    }
}
