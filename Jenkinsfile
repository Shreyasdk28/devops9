pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
    steps {
        sh '''
            # remove old venv to avoid leftover incompatible packages
            rm -rf venv

            python3 -m venv venv
            . venv/bin/activate
            python -m pip install --upgrade pip

            # force reinstall to ensure pinned versions are applied
            pip install --upgrade --force-reinstall -r requirements.txt
            pip list
        '''
    }
}

        stage('Test') {
            steps {
                sh '''
                    . venv/bin/activate
                    python3 -m unittest discover -s .
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    mkdir -p python-app-deploy
                    cp app.py python-app-deploy/
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed. Check logs."
        }
    }
}
