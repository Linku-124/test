pipeline {
    agent any
    
    stages {
        stage('Select Branch') {
            steps {
                script {
                    def gitRepoUrl = 'https://github.com/Linku-124/test.git'  // Replace with your repository URL
                    def branchesOutput = sh(script: "git ls-remote --heads $gitRepoUrl | awk -F'/' '{print \$3}'", returnStatus: true, returnStdout: true)
                    
                    // Check if the sh step succeeded
                    if (branchesOutput == 0) {
                        def branches = branchesOutput.toString().trim()
                        
                        def userInput = input(
                            id: 'branchInput',
                            message: 'Select the branch to build:',
                            parameters: [choice(name: 'BRANCH_NAME', choices: branches.split('\n'), description: 'Select a branch to build')]
                        )
                        
                        echo "Selected branch: ${userInput}"
                    } else {
                        error "Failed to fetch branch names from Git repository."
                    }
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
