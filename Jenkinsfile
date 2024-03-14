pipeline {
        agent {
            label "agent"
        }
        tools {
            jdk 'Java17'
            maven 'Maven3'
        }

        environment {
            APP_NAME = "danko-app"
            RELEASE = "1.0.0"
            DOCKER_USER = "yordo"
            DOCKER_PASS = 'docker'
            IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
            IMAGE_TAG = "${RELEASE}"-"${BUILD_NUMBER}" 
        }

        stages {
            stage ("Cleanup Workspace") {
                steps {
                    cleanWs()
                }
            }

            stage ("Cheackout from SCM") {
                steps {
                    git branch: 'main', credentialsId: 'github', url: 'https://github.com/K1kc4/complete-prodcution-e2e-pipeline.git'
                }
            }

            stage ("Build") {   
                steps {
                    sh "mvn clean package"
                }
            }

            stage ("Test") {
                steps {
                    sh "mvn test"
                }
            }

            stage ("Sonarqube-test") {
                steps {
                    withSonarQubeEnv('sonarqube') {
                        sh "mvn sonar:sonar"
                    }
                }
            }

            stage ("QualityGate") {
                steps {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
               }
            }        

            stage ("Build and push image") {
                steps {
                    docker.withRegistry('', DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }

                    docker.withRegistry('', DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push ('latest')
                    }
                }

            }
    }

}