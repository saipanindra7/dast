pipeline {
    agent any
    environment {
        DASTARDLY_TARGET_URL = 'https://cmrtc.ac.in/'
        IMAGE_WITH_TAG = 'public.ecr.aws/portswigger/dastardly:latest'
        JUNIT_TEST_RESULTS_FILE = 'dastardly-report.xml'
    }
    stages {
        stage("Docker Pull Dastardly from Burp Suite container image") {
            steps {
                script {
                    def dockerPullCmd = "docker pull ${IMAGE_WITH_TAG}"

                    // Use "bat" on Windows, "sh" on Unix-like systems
                    if (isUnix()) {
                        sh script: dockerPullCmd, label: 'unix'
                    } else {
                        bat script: dockerPullCmd, label: 'windows'
                    }
                }
            }
        }
        stage("Docker run Dastardly from Burp Suite Scan") {
            steps {
                script {
                    // Use "bat" on Windows, "sh" on Unix-like systems
                    bat(script: '''
                        docker run --rm --user $(id -u) -v ${WORKSPACE}:${WORKSPACE}:rw ^
                        -e DASTARDLY_TARGET_URL=${DASTARDLY_TARGET_URL} ^
                        -e DASTARDLY_OUTPUT_FILE=${WORKSPACE}/${JUNIT_TEST_RESULTS_FILE} ^
                        ${IMAGE_WITH_TAG}
                    ''', label: 'windows')
                    sh(script: '''
                        docker run --rm --user $(id -u) -v ${WORKSPACE}:${WORKSPACE}:rw \
                        -e DASTARDLY_TARGET_URL=${DASTARDLY_TARGET_URL} \
                        -e DASTARDLY_OUTPUT_FILE=${WORKSPACE}/${JUNIT_TEST_RESULTS_FILE} \
                        ${IMAGE_WITH_TAG}
                    ''', label: 'unix')
                }
            }
        }
    }
    post {
        always {
            junit testResults: "${JUNIT_TEST_RESULTS_FILE}", skipPublishingChecks: true
        }
    }
}
