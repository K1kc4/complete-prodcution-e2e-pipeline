pipeline {
        agent {
            label "agent"
        }
        tools {
            jdk 'Java17'
            maven 'Maven3'
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
        }
}