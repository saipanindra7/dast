pipeline {
    agent any
    environment {
        // ...
    }
    stages {
        stage("Check Docker Installation") {
            steps {
                script {
                    bat(script: 'docker --version', returnStatus: true)
                    bat(script: 'docker info', returnStatus: true)
                }
            }
        }

        // Rest of your stages...
    }

    // ...
}
