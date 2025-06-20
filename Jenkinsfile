pipeline {
    agent any

    environment {
        ACE_PATH = 'C:\\Program Files\\IBM\\ACE\\12.0.12.0\\server\\bin'
        WORKSPACE_DIR = "${env.WORKSPACE}"
    }

    stages {
        stage('Checkout Source Code') {
            steps {
                git branch: 'main', url: 'https://github.com/yashRay-git/IBM-Ace-CICD-BY-jenkins.git'
            }
        }

        stage('Build BAR File') {
            steps {
                bat """
                    call "${ACE_PATH}\\mqsiprofile.cmd"
                    cd "%WORKSPACE%"
                    if exist myApp.bar del myApp.bar
                    mqsicreatebar -data "%WORKSPACE%" -b myApp.bar -a myApp
                """
            }
        }

        stage('Deploy to ACE Server') {
            steps {
                bat """
                    call "${ACE_PATH}\\mqsiprofile.cmd"
                    mqsideploy --integration-node b1 --integration-server server --bar-file myApp.bar --timeout-seconds 300
                """
            }
        }
    }
}
