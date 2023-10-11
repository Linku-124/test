pipeline {
    agent any
    environment {
        GIT_URL = 'https://github.com/Linku-124/test.git'
    }
    stages {
        stage('Select Branch') {
            steps {
                script {
                    // Use 'git ls-remote' to dynamically fetch branch names from the remote repository
                    def branchList = sh(script: "git ls-remote --heads $GIT_URL | awk -F'/' '{print \$3}'", returnStdout: true).trim().split('\n')
                    def userInput = input(
                        id: 'branchInput',
                        message: 'Select the branch to build:',
                        parameters: [choice(name: 'BRANCH_NAME', choices: branchList.join('\n'), description: 'Select a branch to build')]
                    )
                    selectedBranch = userInput ?: 'main' // Set a default branch if userInput is null or empty
                    echo "Selected branch: ${selectedBranch}"
                }
            }
        }
        stage('Checkout and Build') {
            steps {
                script {
                    checkout([$class: 'GitSCM', branches: [[name: selectedBranch]], doGenerateSubmoduleConfigurations: false, userRemoteConfigs: [[url: env.GIT_URL]]])
                    // Replace with your actual build command
                    sh "echo Hi"
                }
            }
        }
    }
    post {
        success {
            echo "Build succeeded!"
        }
        failure {
            echo "Build failed!"
        }
    }
}
