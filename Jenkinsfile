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
            steps {
                
                echo 'deploy'
            }
        }
    }

    post {
        always {
            sh 'deactivate || true'
        }
    }
}
