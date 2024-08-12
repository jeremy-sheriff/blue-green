pipeline {
    agent any

    parameters {
        choice(name: 'ENVIRONMENT', choices: ['blue', 'green'], description: 'Choose the environment to deploy to')
        string(name: 'IMAGE_VERSION', defaultValue: '1.0.1', description: 'Docker image version to deploy')
        booleanParam(name: 'SWITCH_TRAFFIC', defaultValue: false, description: 'Switch traffic to the selected environment')
    }

    stages {
        stage('Deploy to Environment') {
            steps {
                script {
                    if (params.ENVIRONMENT == 'blue') {
                        sh 'kubectl apply -f blue-deployment.yaml'
                        sh "kubectl set image deployment/angular-ui-deployment-blue angular-ui=muhohoweb/angular-ui:${params.IMAGE_VERSION}"
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
                        if (params.ENVIRONMENT == 'blue') {
                            sh 'kubectl apply -f blue/service.yaml'
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
