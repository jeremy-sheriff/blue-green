pipeline {
    agent any

    parameters {
        choice(name: 'ENVIRONMENT', choices: ['blue', 'green'], description: 'Choose the environment to switch traffic to')
    }

    stages {
        stage('Switch Traffic') {
            steps {
                script {
                    // Switch traffic by updating the primary service's selector
                    sh """
                    /opt/homebrew/bin/kubectl patch service angular-ui-service -p '{"spec":{"selector":{"environment":"${params.ENVIRONMENT}"}}}'
                    """
                }
            }
        }
    }
}
