pipeline {
    agent any

    options {
        skipDefaultCheckout(true)   // disable default checkout
    }

    environment {
        GIT_REPO      = "https://github.com/mukeshdevelp/ot-microservice-sarthi.git"
        GIT_BRANCH    = "backend"
        PROJECT_DIR   = "salary/salary-api"
        MAVEN_TOOL    = "Maven3"
        SLACK_CHANNEL = "#ci-operation-notifications"
    }

    tools {
        maven "${MAVEN_TOOL}"
    }

    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout Code') {
            steps {
                git url: "${GIT_REPO}", branch: "${GIT_BRANCH}"
            }
        }

        stage('Unit Testing') {
            steps {
                dir("${PROJECT_DIR}") {
                    catchError(buildResult: 'SUCCESS', stageResult: 'UNSTABLE') {
                        sh "mvn test"
                    }
                }
            }
        }
    }

    post {

        success {
            slackSend(
                channel: "${SLACK_CHANNEL}",
                color: 'good',
                message: """
Build Successful

Job: ${env.JOB_NAME}
Build: #${env.BUILD_NUMBER}
URL: ${env.BUILD_URL}
"""
            )
        }

        failure {
            slackSend(
                channel: "${SLACK_CHANNEL}",
                color: 'danger',
                message: """
Build Failed

Job: ${env.JOB_NAME}
Build: #${env.BUILD_NUMBER}
URL: ${env.BUILD_URL}
"""
            )
        }

        always {
            archiveArtifacts artifacts: '**/reports/*', fingerprint: true
        }
    }
}
