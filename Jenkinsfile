pipeline {
    agent any

    stages {
        stage('Authenticate to AWS') {
            steps {
                script {
                    echo 'Authenticating to AWS...'

                    // Use AWS Credentials Plugin to inject credentials
                    withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'AWS_ACCESS', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {

                        sh '''
                        export AWS_DEFAULT_REGION=eu-west-1

                        echo "AWS authentication in progress..."
                        
                        # Confirm AWS authentication
                        aws sts get-caller-identity
                        '''
                    }
                }
            }
        }

        stage('Terraform Init') {
            steps {
                script {
                    echo 'Initializing Terraform...'
                    sh '''
                    terraform init
                    '''
                }
            }
        }
        
        stage('Terraform Plan') {
            steps {
                script {
                    sh '''
                        echo 'Running Terraform Plan...'
                        terraform plan -var-file="terraform.tfvars" --auto-approve -out=tfplan 
                    '''
                }
            }
        }
        
        stage('Terraform Apply') {
            steps {
                script {
                    sh '''
                         echo 'Applying Terraform Plan...'
                         terraform apply -var-file="terraform.tfvars" --auto-approve -out=tfplan
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}
