pipeline {
    agent any

    environment {
        PATH = "$PATH:/usr/local/bin"
    }

    parameters {
        choice(name: 'ENVIRONMENT', choices: ['blue', 'green'], description: 'Choose the environment to deploy to')
        string(name: 'API_IMAGE', defaultValue: '1.0.8', description: 'Docker image version for the API to deploy')
        string(name: 'UI_IMAGE', defaultValue: '1.0.8', description: 'Docker image version for the UI to deploy')
        booleanParam(name: 'SWITCH_TRAFFIC', defaultValue: false, description: 'Switch traffic to the selected environment')
    }

    stages {

        stage('Deploy to Environment') {
            steps {
                script {
                    def tempFile = "/tmp/transformed_ui.yaml"
                    if (params.ENVIRONMENT == 'blue') {
                        // Write the transformed YAML to a temporary file
                        sh """
                        sed 's/{{ENVIRONMENT}}/${params.ENVIRONMENT}/g; s/{{UI_IMAGE}}/${params.UI_IMAGE}/g' /Users/jeremy/work_dir/blue-green/blue/deployment/ui.yaml > ${tempFile}
                        """
                        // Display the content of the transformed YAML
                        sh "cat ${tempFile}"
                        // Apply the transformed YAML using kubectl
                        sh "kubectl apply -f ${tempFile}"
                    } else {
                        // Handle green environment similarly
                        sh """
                        sed 's/{{ENVIRONMENT}}/${params.ENVIRONMENT}/g; s/{{UI_IMAGE}}/${params.UI_IMAGE}/g' /Users/jeremy/work_dir/blue-green/green/deployment/ui.yaml > ${tempFile}
                        """
                        sh "cat ${tempFile}"
                        sh "kubectl apply -f ${tempFile}"
                    }
                }
            }
        }

        stage('Switch Traffic') {
            steps {
                script {
                    def tempServiceFile = "/tmp/transformed_ui_service.yaml"
                    if (params.SWITCH_TRAFFIC) {
                        echo 'Switching traffic to the selected environment...'
                        if (params.ENVIRONMENT == 'blue') {
                            sh """
                            sed 's/{{ENVIRONMENT}}/${params.ENVIRONMENT}/g;' /Users/jeremy/work_dir/blue-green/services/ui-service.yaml > ${tempServiceFile}
                            """
                            sh "cat ${tempServiceFile}"
                            sh "kubectl apply -f ${tempServiceFile}"
                        } else {
                            sh """
                            sed 's/{{ENVIRONMENT}}/${params.ENVIRONMENT}/g;' /Users/jeremy/work_dir/blue-green/services/ui-service.yaml > ${tempServiceFile}
                            """
                            sh "cat ${tempServiceFile}"
                            sh "kubectl apply -f ${tempServiceFile}"
                        }
                    } else {
                        echo 'Traffic switch not requested.'
                    }
                }
            }
        }
    }
}
