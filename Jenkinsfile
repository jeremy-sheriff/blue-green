pipeline {
    agent any

    environment {
        PATH = "$PATH:/usr/local/bin"
        TEST_PATH =  '/Users/jeremy/work_dir/blue-green/tests'
    }

    parameters {
        choice(name: 'ENVIRONMENT', choices: ['blue', 'green'], description: 'Choose the environment to deploy to')
        string(name: 'API_IMAGE', defaultValue: '2.0.0', description: 'Docker image version for the API to deploy')
        string(name: 'UI_IMAGE', defaultValue: '2.0.6', description: 'Docker image version for the UI to deploy')
    }

    stages {

        stage('Run Postman Tests') {
            steps {
                script {
                    // Use the global variable for the collection path
                    def result = sh(script: "/opt/homebrew/bin/newman run ${env.TEST_PATH}/good_collection.json -e ${env.TEST_PATH}/env.json", returnStatus: true)

                    // Check if tests failed
                    if (result != 0) {
                        error "Postman tests failed. Stopping the pipeline."
                    }
                }
            }
        }


        stage('Deploy') {
            steps {
                script {
                    sh """
                    /opt/homebrew/bin/helm upgrade --install angular-ui-${params.ENVIRONMENT} . \
                    --set ui.image.tag=${params.IMAGE_TAG} \
                    --set ui.environment=${params.ENVIRONMENT}
                    """
                }
            }
        }

//        stage('Deploy to Environment') {
//            steps {
//                script {
//                    def tempFile = "/tmp/transformed_ui.yaml"
//                        sh "pwd"
//                        sh """
//                        sed 's/{{ENVIRONMENT}}/${params.ENVIRONMENT}/g; s/{{UI_IMAGE}}/${params.UI_IMAGE}/g' /Users/jeremy/work_dir/blue-green/ui.yaml > ${tempFile}
//                        """
//                        sh "cat ${tempFile}"
//                        sh "kubectl apply -f ${tempFile}" //1,2
//                }
//            }
//        }
    }
}
