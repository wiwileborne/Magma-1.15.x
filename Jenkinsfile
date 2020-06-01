@Library('forge-shared-library')_

pipeline {
    agent {
        docker { image 'openjdk:8-jdk' }
    }
    environment {
        DISCORD_PREFIX = "Magma: ${BRANCH_NAME} #${BUILD_NUMBER}"
    }
    stages {
        stage('Setup') {
         agent {
                label 'windows'
            }
            steps {
                withCredentials([string(credentialsId: 'DISCORD_WEBHOOK', variable: 'discordWebhook')]) {
                    //discordSend(
                    //        title: "${DISCORD_PREFIX} Started",
                    //        successful: true,
                    //        result: 'ABORTED', //White border
                    //        thumbnail: "https://img.hexeption.co.uk/Magma_Block.png",
                    //        webhookURL: "${discordWebhook}"
                    //)
                }
                sh 'chmod +x gradlew'
            }
        }

        stage('Build') {
          agent {
              label 'windows'
          }
            steps {
                sh './gradlew setup installerJar --console=plain'
            }
        }
    }
    post {
        always {
            script {
                archiveArtifacts artifacts: 'projects/magma/build/libs/*', fingerprint: true, onlyIfSuccessful: true, allowEmptyArchive: true
                withCredentials([string(credentialsId: 'DISCORD_WEBHOOK', variable: 'discordWebhook')]) {
                   //discordSend(
                   //        title: "Finished ${currentBuild.currentResult}",
                   //        description: '```\n' + getChanges(currentBuild) + '\n```',
                   //        successful: currentBuild.resultIsBetterOrEqualTo("SUCCESS"),
                   //        result: currentBuild.currentResult,
                   //        thumbnail: "https://img.hexeption.co.uk/Magma_Block.png",
                   //        webhookURL: "${discordWebhook}"
                   //)
                }
            }
        }
    }
}

