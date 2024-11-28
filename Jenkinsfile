pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '2ec10204-01f0-4172-8443-031d023cf50d'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {

        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }

        stage('Tests') {
            parallel {
                stage('Unit tests') {
                    agent {
                        docker {
                            image 'node:18-alpine'
                            reuseNode true
                        }
                    }

                    steps {
                        sh '''
                            #test -f build/index.html
                            npm test
                        '''
                    }
                 }

                stage('E2E') {
                    steps {
                        sh '''
                        echo 'this is E2E test'
                        echo 'E2E test completed'
                        '''
                    }

                  }
            }
        }

        stage('Deploy staging') {
             environment {
                CI_ENVIRONMENT_URL = 'STAGING_URL_TO_BE_SET'
            }

            steps {
                       sh '''
                        echo 'this is Staging'
                        echo 'Staging test completed'
                        '''
            }
        }

        stage('Approval') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    input message: 'Do you wish to deploy to production?', ok: 'Yes, I am sure!'
                }
            }
        }

        stage('Deploy prod') {
             environment {
                CI_ENVIRONMENT_URL = '2ec10204-01f0-4172-8443-031d023cf50d'
            }

            steps {
                       sh '''
                        echo 'this is prod'
                        echo 'prod completed...'
                        '''
            }


        }
    }
}
