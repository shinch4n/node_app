pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'sq1' // Name of the SonarQube server configuration in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Test') {
            steps {
                sh 'npm install'
                sh 'npm run build'
                sh 'npm test'
            }
        }

        stage('SonarQube Scan') {
            steps {
                script {
                    def scannerHome = tool name: 'SonarQube', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    withSonarQubeEnv(SONARQUBE_SERVER) {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
        // stage('SonarQube Analysis') {
        //     def scannerHome = tool 'SonarScanner';
        //     withSonarQubeEnv() {
        //       sh "${scannerHome}/bin/sonar-scanner"
        //     }
        //  }

        stage('Deploy') {
            when {
                expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
            }
            steps {
                // Deploy your Node.js application here
                sh 'npm start'
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
    node {
        stage('SCM') {
            checkout scm
          }
        stage('SonarQube Analysis') {
            def scannerHome = tool 'SonarScanner';
            withSonarQubeEnv() {
              sh "${scannerHome}/bin/sonar-scanner"
        }
    }
}
