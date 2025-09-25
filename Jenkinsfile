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
    steps {
        sh '''
        set -e
        # Copy project files
        rsync -av --delete . /opt/flaskapp/

        cd /opt/flaskapp
        python3 -m venv venv
        venv/bin/pip install --upgrade pip
        venv/bin/pip install -r requirements.txt

        # Restart service
        sudo systemctl restart flaskapp
        '''
    }
}
    }

   
// Replace the pipeline above, keep stages same, and use this post block instead:
post {
    success {
        mail to: 'aish.p411@gmail.com',
             subject: "SUCCESS: ${currentBuild.fullDisplayName}",
             body: "Build succeeded: ${env.BUILD_URL}"
    }
    failure {
        mail to: 'aish.p411@gmail.com',
             subject: "FAILURE: ${currentBuild.fullDisplayName}",
             body: "Build failed: ${env.BUILD_URL}\nSee console output for details."
    }
}

}
