pipeline {
    agent any

    environment {
        WORKSPACE_DIR = '/var/lib/jenkins/workspace/adqgpt'
        PATH = "/usr/local/bin:$PATH"  // Ensure Python and pip are in the PATH
    }

    stages {
        stage('Checkout') {
            steps {
                dir("${WORKSPACE_DIR}") {
                    git branch: 'main', url: 'https://github.com/Dineshkundo/flask.git'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                dir("/var/lib/jenkins/workspace/adqgpt") {
                    sh '''
                    sudo apt-get update
                    sudo apt install -y python3-pip
                    sudo apt install -y python3.8-venv tmux
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install -r requirement.txt
                    pip install gunicorn
                    '''
                }
            }
        }

        stage('Run gunicorn') {
            steps {
                dir("${WORKSPACE_DIR}") {
                    sh '''
                    . venv/bin/activate
                    tmux new-session -d -s gunicorn_session 'gunicorn --bind 0.0.0.0:5000 run:app'
                    tmux ls
                    '''
                }
            }
        }
    }
}
