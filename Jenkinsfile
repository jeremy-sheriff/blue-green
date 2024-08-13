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

        stage('Preview Transformed YAML') {
            steps {
                script {
                    def tempFile = "/tmp/transformed_ui.yaml"
                    if (params.ENVIRONMENT == 'blue') {
                        // Write the transformed YAML to a temporary file
                        sh """
                        sed 's/{{ENVIRONMENT}}/blue/g; s/{{UI_IMAGE}}/muhohoweb\\/angular-ui:1.0.8/g' /path/to/your/ui.yaml > /path/to/output.yaml
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

//        stage('Deploy to Environment') {
//            steps {
//                script {
//                    if (params.ENVIRONMENT == 'blue') {
//                        // Replace placeholders in the BLUE deployment YAML file with the actual parameter values from Jenkins
//                        sh """
//                        sed 's/{{ENVIRONMENT}}/${params.ENVIRONMENT}/g; s/{{UI_IMAGE}}/${params.UI_IMAGE}/g' /Users/jeremy/work_dir/blue-green/blue/deployment/ui.yaml | kubectl apply -f -
//                        """
//                    } else {
//                        // Replace placeholders in the GREEN deployment YAML file with the actual parameter values from Jenkins
//                        sh """
//                        sed 's/{{ENVIRONMENT}}/${params.ENVIRONMENT}/g; s/{{UI_IMAGE}}/${params.UI_IMAGE}/g' /Users/jeremy/work_dir/blue-green/green/deployment/ui.yaml | kubectl apply -f -
//                        """
//                    }
//                }
//            }
//        }

//        stage('Switch Traffic') {
//            steps {
//                script {
//                    if (params.SWITCH_TRAFFIC) {
//                        echo 'Switching traffic to the selected environment...'
//                        if (params.ENVIRONMENT == 'BLUE') {
//                            // Apply the service configuration for the BLUE environment
//                            sh 'kubectl apply -f /Users/jeremy/work_dir/blue-green/blue/ui-service.yaml'
//                        } else {
//                            // Apply the service configuration for the GREEN environment
//                            sh 'kubectl apply -f /Users/jeremy/work_dir/blue-green/green/ui-service.yaml'
//                        }
//                    } else {
//                        echo 'Traffic switch not requested.'
//                    }
//                }
//            }
//        }
    }
}
