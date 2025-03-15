pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', credentialsId: 'your-git-credentials', url: 'https://github.com/rk4027-N/Myfirstrepo.git'
            }
        }
        stage('Build') {
            steps {
                sh 'echo "<html><body><h1>Test Report</h1></body></html>" > report.html'
            }
        }
        stage('Publish HTML Report') {
            steps {
                publishHTML([
                    target: [
                        allowMissing           : false,
                        alwaysLinkToLastBuild : false,
                        keepAll                : false,
                        reportDir              : '.',
                        reportFiles            : 'report.html',
                        reportName             : 'My HTML Report'
                    ]
                ])
            }
        }
    }
}
