node {

    /* -------------------------------
       Global Variables
    --------------------------------*/

    def GIT_REPO    = "https://github.com/mukeshdevelp/ot-microservice-sarthi.git"
    def GIT_BRANCH  = "backend"
    def PROJECT_DIR = "salary/salary-api"

    def MAVEN_TOOL  = "Maven3"
    def SLACK_CHANNEL = "#ci-operation-notifications"

    def mvnHome

    try {

        stage('Checkout Code') {
            git url: GIT_REPO, branch: GIT_BRANCH
        }

        stage('Initialize Tools') {
            mvnHome = tool MAVEN_TOOL
        }

        stage('Unit Testing') {
            dir(PROJECT_DIR) {
                catchError(buildResult: 'SUCCESS', stageResult: 'UNSTABLE') {
                    sh "${mvnHome}/bin/mvn test"
                }
            }
        }

        echo "Unit Testing completed."

        /* -------- SUCCESS NOTIFICATION -------- */
        slackSend(
            channel: SLACK_CHANNEL,
            color: 'good',
            message: """
Build Successful

Job: ${env.JOB_NAME}
Build: #${env.BUILD_NUMBER}
URL: ${env.BUILD_URL}
"""
        )

    } catch (err) {

        /* -------- FAILURE NOTIFICATION -------- */
        slackSend(
            channel: SLACK_CHANNEL,
            color: 'danger',
            message: """
Build Failed

Job: ${env.JOB_NAME}
Build: #${env.BUILD_NUMBER}
URL: ${env.BUILD_URL}
"""
        )

        error "Pipeline failed: ${err}"

    } finally {

        /* -------- POST ACTION -------- */
        archiveArtifacts artifacts: '**/reports/*', fingerprint: true

    }
}
