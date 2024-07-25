pipeline {
    agent any

    environment {
        MAVEN_HOME = tool 'Maven-3.9.0' // Ensure this matches your Maven tool name
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from the GitHub repository
                script {
                    def branchName = env.BRANCH_NAME
                    echo "Checking out branch: ${branchName}"
                    git url: 'https://github.com/nkheria/DevOpsClassCodes.git', branch: branchName
                }
            }
        }

        stage('Build') {
            steps {
                // Build the project using Maven
                script {
                    withEnv(["PATH+MAVEN=${MAVEN_HOME}/bin"]) {
                        sh 'mvn clean package'
                    }
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                // Archive the built artifacts
                archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
            }
        }

        stage('Test') {
            when {
                expression { return env.BRANCH_NAME.startsWith('feature/') }
            }
            steps {
                // Run tests for feature branches
                script {
                    withEnv(["PATH+MAVEN=${MAVEN_HOME}/bin"]) {
                        sh 'mvn test'
                    }
                }
            }
        }

        stage('Deploy') {
            when {
                branch 'master'
            }
            steps {
                // Deploy for the master branch
                echo 'Deploying to production...'
                // Add your deployment commands here
                // e.g., sh 'deploy.sh'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Pipeline succeeded.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
