pipeline {
    agent any

    environment {
            PATH = "$PATH:/usr/local/bin"
        }

    parameters {
        choice(name: 'ENVIRONMENT', choices: ['BLUE', 'GREEN'], description: 'Choose the environment to deploy to')
        string(name: 'API_IMAGE', defaultValue: '1.0.8', description: 'Docker image version for the API to deploy')
        string(name: 'UI_IMAGE', defaultValue: '1.0.8', description: 'Docker image version for the UI to deploy')
        booleanParam(name: 'SWITCH_TRAFFIC', defaultValue: false, description: 'Switch traffic to the selected environment')
    }


    stages {
        stage('Deploy to Environment') {
            steps {
            //kubectl apply -f blue/deployment/ui.yaml
                script {
                    if (params.ENVIRONMENT == 'BLUE') {
                     script {
                        // Replace the placeholder in the YAML file with the actual IMAGE_VERSION from Jenkins parameter
                        sh """
                        sed 's/{{ENVIRONMENT}}/${params.ENVIRONMENT}/g; s/{{UI_IMAGE}}/${params.UI_IMAGE}/g' k8s/angular-ui-deployment.yaml | kubectl apply -f -
                        """
                       }
                    } else {
                        sh 'kubectl apply -f green-deployment.yaml'
                        sh "kubectl set image deployment/angular-ui-deployment-green angular-ui=muhohoweb/angular-ui:${params.IMAGE_VERSION}"
                    }
                }
            }
        }

        stage('Switch Traffic') {
            steps {
                script {
                    if (params.SWITCH_TRAFFIC) {
                    //kubectl apply -f blue/service.yaml
                        if (params.ENVIRONMENT == 'blue') {
                            sh ''
                        } else {
                            sh 'kubectl apply -f green/service.yaml'
                        }
                    } else {
                        echo 'Traffic switch not requested.'
                    }
                }
            }
        }
    }
}
