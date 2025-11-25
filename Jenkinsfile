pipeline {
    agent any

    tools {
        nodejs 'node20'
    }

    stages {
        
        stage('Lint') {
            steps {
                bat 'npm run lint || exit 0'
            }
        }

        stage('Install') {
            steps {
                bat 'npm install'
            }
        }

        stage('Unit Tests') {
            steps {
                bat 'npm run test --if-present'
            }
        }

        stage('Build') {
            steps {
                bat 'npm run build'
            }
        }

        stage('Versioning') {
            steps {
                script {
                    env.APP_VERSION = "v1.0.${BUILD_NUMBER}"
                    echo "Generated Version: ${env.APP_VERSION}"
                }
            }
        }

       stage('Git Tag') {
            steps {
                bat """
                    git config user.email "jenkins@localhost"
                    git config user.name "Jenkins"
                    git tag -a ${APP_VERSION} -m "Build ${APP_VERSION}"
                    git push origin ${APP_VERSION}
                """
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: 'dist/jenkins-angular-demo/**'
        }
    }
}
