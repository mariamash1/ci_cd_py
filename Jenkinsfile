pipeline {
    agent any

    stages {
        stage('Clone GitHub Repo') {
            steps {
                checkout scm
            }
        }
        stage('Setup Python Virtual Environment') {
            steps {
                script {
                    if (!fileExists('venv')) {
                        sh 'python3 -m venv venv --system-site-packages'
                    }
                    // Attempt to upgrade pip, install if not found
                    try {
                        sh './venv/bin/python -m pip install --upgrade pip'
                    } catch (any) {
                        sh 'curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py'
                        sh './venv/bin/python get-pip.py'
                    }
                }
            }
        }
         stage('Install Dependencies') {
            steps {
                sh '''
                    . venv/bin/activate
                    pip install -r requirements.txt
                '''
            }
        }       
        stage('Run Tests') {
            steps {
                // Direct use of pytest from within the virtual environment
                sh './venv/bin/pytest test11.py'
            }
        }
        stage('Update Remote Repository') {
            steps {
                script {
                    // Merge the 'dev' branch into the 'main' branch
                    GITHUB_URL = "https://github.com/mariamash1/ci_cd_pytest.git"
                    GITHUB_BRANCH = "main"
                    GITHUB_MERGE_BRANCH = "dev"

                    sh(script: """
                        git config --global user.email "your-email@example.com"
                        git config --global user.name "Your Name"
                        git checkout ${GITHUB_BRANCH}
                        git merge ${GITHUB_MERGE_BRANCH}
                        git push https://${GITHUB_CREDENTIALS}@${GITHUB_URL} ${GITHUB_BRANCH}
                    """, returnStdout: true)
                }
            }
        }
    }
}
