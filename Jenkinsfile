pipeline {
    agent none  
	options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') { 
            agent {
                docker {
                    image 'python:2-alpine' 
                }
            }
            steps {
                sh 'python -m py_compile administrator.py' 
                stash(name: 'compiled-results', includes: '*.py*') 
            }
        }
		stage('Deliver') { 
            agent {
                docker {
                    image 'cdrx/pyinstaller-linux:python2'
                }
            }
            steps {
                sh 'pyinstaller --onefile administrator.py'
            }
            post {
                success {
                    archiveArtifacts 'dist/administrator'
                }
            }
        }
    }
}
