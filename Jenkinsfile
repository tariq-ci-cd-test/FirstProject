pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
        PIP_CACHE_DIR = "${HOME}/.cache/pip"
    }

    stages {
        stage('Checkout') {
            steps {
                // Pull code from GitHub
                checkout scm
            }
        }

        stage('Setup Python') {
            steps {
                sh "python3 -m venv ${VENV_DIR}"
                sh ". ${VENV_DIR}/bin/activate && pip install --upgrade pip"
            }
        }

        stage('Cache Dependencies') {
            steps {
                // Create cache directory if it doesn't exist
                sh "mkdir -p ${PIP_CACHE_DIR}"
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                . venv/bin/activate
                if [ -f requirements.txt ]; then
                    echo "Installing dependencies..."
                    pip install --cache-dir $HOME/.cache/pip -r requirements.txt
                else
                    echo "No dependencies to install"
                fi
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                . venv/bin/activate
                echo "Running tests..."
                # If you add tests, install pytest and run them
                if [ -f test_main.py ]; then
                    pip install pytest
                    pytest
                else
                    echo "No test file found"
                fi
                '''
            }
        }

        stage('Deploy (Optional)') {
            steps {
                echo "Deploy stage placeholder. Add deployment commands here."
                // Example: scp files to server, Docker build, Heroku deploy
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished!'
        }
        success {
            echo 'All stages passed!'
        }
        failure {
            echo 'Something went wrong!'
        }
    }
}
