pipeline {
    agent any

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

        stage('Debug Branch') {
            steps {
                sh 'git branch'
                sh 'pwd'
                sh 'ls -la'
            }
        }
    }

    post {
        success {
            slackSend(
                channel: "${SLACK_CHANNEL}",
                color: 'good',
                message: "Build Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
            )
        }

        failure {
            slackSend(
                channel: "${SLACK_CHANNEL}",
                color: 'danger',
                message: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
            )
        }

        always {
            archiveArtifacts artifacts: '**/reports/*', fingerprint: true
        }
    }
}
