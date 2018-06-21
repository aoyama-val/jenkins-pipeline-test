#!groovy

def err_msg = ""

node {
    try {
        stage '第一ステージ' 
        sh 'echo Hello Pipeline.'

        stage '第二ステージ'
        sh '''
            date
            uname -a
            id
            exit 0
           '''
    } catch (err) {
        err_msg = "${err}"
        currentBuild.result = "FAILURE"
    } finally {
        if (currentBuild.result != "FAILURE") {
            currentBuild.result = "SUCCESS"
        }
        notification(err_msg)
    }
}

// 実行結果のSlack通知
def notification(msg) {
    def slack_channel = "#times_aoyama"  // jenkinsが通知するチャネル
    def slack_domain = ""           // slackのドメイン名 https://mydomain.slack.comのmydomainの部分
    def slack_token = ""            // slackのjenkinsプラグインで取得できるtoken
    def slack_color = "good"
    def slack_icon = ""
    def detail_link = "(<${env.BUILD_URL}|Open>)"  // SlackでOpenのアンカーとして表示されます
    // ビルドエラー時にメッセージの装飾を行う
    if(currentBuild.result == "FAILURE") {
        slack_color = "danger"
    }
    def slack_msg = "${env.JOB_NAME} - #${env.BUILD_NUMBER} ${currentBuild.result} after ${currentBuild.durationString} ${detail_link} \n\n ${msg}"
    slackSend channel: "${slack_channel}", color: "${slack_color}", message: "${slack_msg}", teamDomain: "${slack_domain}", token: "${slack_token}"
}