pipeline {
    agent any

    environment {
        VENV_DIR = ".venv"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Setup Python') {
            steps {
                sh 'python3 -m venv $VENV_DIR'
                sh './$VENV_DIR/bin/pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh './$VENV_DIR/bin/pytest'
            }
        }
        stage('new Test') {
            steps {
                sh './$VENV_DIR/bin/pytest tests/test_new.py'
            }
        }
    }

    post {
        always {
            echo "Pipeline completed."
        }
    }
}
