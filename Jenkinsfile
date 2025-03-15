pipeline {
    agent any
    tools {
        maven 'Maven 3.8.6' // Or your Maven tool name if different
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', credentialsId: 'my-git-credentials', url: 'https://github.com/rk4027-N/Myfirstrepo.git'
            }
        }
        stage('Build and Test') {
            steps {
                sh 'mvn clean install -DskipTests=false'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Static Analysis') {
            steps {
                sh 'mvn pmd:pmd checkstyle:checkstyle'
            }
            post {
                always {
                    recordIssues tool: checkstyle(pattern: 'target/checkstyle-reports/checkstyle.xml'), unstableTotalHigh: 10, unstableTotalNormal: 20
                    recordIssues tool: pmdParser(pattern: 'target/pmd.xml'), unstableTotalHigh: 10, unstableTotalNormal: 20
                }
            }
        }
        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }
        stage('Docker Build') {
            agent {
                docker {
                    image 'docker:latest'
                    args '-v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            steps {
                script {
                    def imageName = "your-docker-registry/your-app:${BUILD_NUMBER}"
                    sh "docker build -t ${imageName} ."
                    sh "docker push ${imageName}"
                    env.DOCKER_IMAGE = imageName
                }
            }
        }
        stage('Deploy to Development') {
            steps {
                script {
                    sh "echo 'Deploying ${env.DOCKER_IMAGE} to development...'"
                    // Replace with your actual deployment commands
                    sh "echo 'Deployment commands here, using ${env.DOCKER_IMAGE}'"
                }
            }
        }
        stage('Notify') {
            steps {
                script {
                    if (currentBuild.result == 'SUCCESS') {
                        echo "Build successful!"
                        // Send success notification
                    } else {
                        echo "Build failed!"
                        // Send failure notification
                    }
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
