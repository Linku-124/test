pipeline {
    agent any
    
    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }
        
        stage('Select Branch') {
            steps {
                script {
                    def gitRepoUrl = 'https://github.com/Linku-124/test.git'  // Replace with your repository URL
                    def branchesOutput = sh(script: "git ls-remote --heads $gitRepoUrl | awk -F'/' '{print \$3}'", returnStdout: true).trim()
                    
                    // Split the branch names into an array
                    def branches = branchesOutput.split('\n')
                    
                    // Create a list of choice parameters
                    def branchChoices = branches.collect { branchName ->
                        return choice(name: branchName, value: branchName)
                    }
                    
                    def userInput = input(
                        id: 'branchInput',
                        message: 'Select the branch to build:',
                        parameters: branchChoices
                    )
                    
                    echo "Selected branch: ${userInput}"
                }
            }
        }
        
        stage('Checkout and Build') {
            steps {
                script {
                    def selectedBranch = env.BRANCH_NAME
                    checkout([$class: 'GitSCM', branches: [[name: selectedBranch]], doGenerateSubmoduleConfigurations: false, extensions: []])
                    sh "your-build-command-here"  // Replace with your build command
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
