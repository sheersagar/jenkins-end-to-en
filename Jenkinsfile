pipeline {
    agent {
        label "jenkins-agent"
    }

    tools {
        jdk 'Java17'
        maven 'Maven3'
    }

    stages{
        stage ("Clean up Workspae") {
            steps{
                cleanWs()
            }
        }


        stage ("Checkout from SCM") {
            steps{
                git branch: 'main', credentialsId: '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', url: 'https://github.com/sheersagar/jenkins-end-to-en.git'
            }
        }
    }
}