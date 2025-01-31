pipeline {
        agent any

        environment {
                SONAR_PROJECT_KEY = 'node-project'
		SONAR_SCANNER_HOME = tool 'SonarQubeScanner'
        }

        stages {
                stage('Checkout Github'){
                        steps {
                                git branch: 'main', credentialsId: 'git-token', url: 'https://github.com/jaladeesh/Simple_NodeJS_App.git'
                        }
                }

                stage('Install node dependencies'){
                        steps {
                                sh 'npm install'
                        }
                }
                stage('Tests'){
                        steps {
                                sh 'npm test'
                        }
                }
                stage('SonarQube Analysis'){
                        steps {
                                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                                   
                                        withSonarQubeEnv('SonarQube') {
                                                sh """
                                                ${SONAR_SCANNER_HOME}/bin/sonar-scanner \
                                                -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                                                -Dsonar.sources=. \
                                                -Dsonar.host.url=http://35.154.100.80:9000/ \
                                                -Dsonar.login=${SONAR_TOKEN}
                                                """
                                        }
                                }
                        }
                }
        }
        post {
                success {
                        echo 'Build completed succesfully!'
                }
                failure {
                        echo 'Build failed. Check logs.'
                }
        }
}
