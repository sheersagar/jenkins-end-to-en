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

        stage("Build Application") {
            steps{
                sh "mvn clean package"
            }
        }

        stage("Test Application") {
            steps{
                sh "mvn test"
            }
        }

        // For Sonar Qube analysis

        stage("Sonar Qube Analysis") {
            steps{
                // the credentials ID will be the same name that we setup in jenkins credentials
                withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') {
                    sh "mvn sonar:sonar"
                }
            }
        }
    }
}