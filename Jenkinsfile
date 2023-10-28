pipeline {
    agent {
        label "jenkins-agent"
    }

    tools {
        // these are the tools that we will use to buil our code
        jdk 'Java17'
        maven 'Maven3'
    }

    stages{

        // It is the stage that will clear out our workspace
        stage ("Clean up Workspae") {
            steps{
                cleanWs()
            }
        }


        stage ("Checkout from SCM") {
            // It will checkout our git repo code
            steps{
                git branch: 'main', credentialsId: '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', url: 'https://github.com/sheersagar/jenkins-end-to-en.git'
            }
        }

        stage("Build Application") {

            // It will build our Application with the Code that is present in the repo
            steps{
                sh "mvn clean package"
            }
        }

        stage("Test Application") {
            steps{
                sh "mvn test"
            }
        }
    }
}