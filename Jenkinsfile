pipeline {
    agent {
        label "jenkins-agent"
    }

    tools {
        jdk 'Java17'
        maven 'Maven3'
    }

    environment {
        APP_NAME = "jenkins-en-to-en"
        RELEASE = "1.0.0"
        DOCKER_USER = "vishv3432"
        DOCKER_PASS = 'dockerhub'
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
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
                script {
                    // the credentials ID will be the same name that we setup in jenkins credentials
                    withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') {
                        sh "mvn sonar:sonar"
                    }
                }
            }  
        }


        stage("Quality Gate") {
            steps {
                script {
                    waitForQualityGate(abortPipeline: false, credentialsId: 'jenkins-sonarqube-token')
                }
            }  
        }

        stage("Docker build and Push") {
            steps {
                script {
                    docker.withDockerRegistry('', DOCKER_PASS) {
                        docker_image = docker.build"${IMAGE_NAME}"
                    }

                    docker.withDockerRegistry('', DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }  
        }

    }
}