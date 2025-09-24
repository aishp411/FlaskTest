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

        stage('Deploy') {
            when {
                expression { return env.BRANCH_NAME == null || env.BRANCH_NAME == 'main' }
            }
            steps {
                sh '''
                echo "Deploy: syncing workspace to $DEPLOY_DIR"
                rsync -a --delete "$WORKSPACE/" "$DEPLOY_DIR/"

                echo "Creating venv in deploy folder if missing"
                python3 -m venv $DEPLOY_DIR/venv || true

                echo "Installing dependencies in deploy venv"
                . $DEPLOY_DIR/venv/bin/activate
                pip install --upgrade pip
                pip install -r $DEPLOY_DIR/requirements.txt

                echo "Restarting systemd service"
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
