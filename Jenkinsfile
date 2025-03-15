pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                // Your build steps here (generating HTML reports)
                sh 'echo "<html><body><h1>Test Report</h1></body></html>" > report.html'
            }
        }
        stage('Publish HTML Report') {
            steps {
                publishHTML(
                    [allowMissing = false,
                     alwaysLinkToLastBuild = false,
                     keepAll = false,
                     reportDir = '.',
                     reportFiles = 'report.html',
                     reportName = 'My HTML Report']
                )
            }
        }
    }
}
