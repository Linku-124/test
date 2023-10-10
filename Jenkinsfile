pipeline {
    agent any
    
    parameters {
        extendedChoice(
            name: 'BRANCH_NAME',
            description: 'Select a branch to build:',
            type: 'PT_SINGLE_SELECT',
            groovyScript: '''
                def gitRepoUrl = 'https://github.com/Linku-124/test.git'
                def branchesOutput = sh(script: "git ls-remote --heads $gitRepoUrl | awk -F'/' '{print \$3}'", returnStdout: true).trim()
                def branches = branchesOutput.split('\n')
                return branches
            '''
        )
    }
    
    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }
        
        stage('Checkout and Build') {
            steps {
                script {
                    def selectedBranch = params.BRANCH_NAME
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
