pipeline {
    agent any

    environment {
        PATH = "$PATH:/usr/local/bin"
        TEST_PATH =  '/Users/jeremy/work_dir/blue-green/tests'
    }

    parameters {
        choice(name: 'ENVIRONMENT', choices: ['blue', 'green'], description: 'Choose the environment to deploy to')
        booleanParam(name: 'SWITCH_TRAFFIC', defaultValue: true, description: 'Switch traffic to the selected environment')
    }

    stages {

        stage('Switch Traffic') {
            steps {
                script {
                    def tempServiceFile = "/tmp/transformed_ui_service.yaml"
                    if (params.SWITCH_TRAFFIC) {
                        echo 'Switching traffic to the selected environment...'
                        if (params.ENVIRONMENT == 'blue') {
                            sh "ls -l"
                            sh "pwd"
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
