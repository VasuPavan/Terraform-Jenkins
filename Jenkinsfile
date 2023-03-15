pipeline {

    parameters {
        booleanParam(defaultValue: true, description: 'Create AWS Infrastructure', name: 'apply')
        booleanParam(defaultValue: true, description: 'Destroy the AWS Infrastructure', name: 'destroy')
    } 
    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }

   agent  any
    stages {
        stage('checkout') {
            steps {
                 script{
                        dir("terraform")
                        {
                            git "https://github.com/VasuPavan/Terraform-Jenkins.git"
                        }
                    }
                }
            }

        stage('Infra-Init/Plan') {
            steps {
                sh 'pwd;cd terraform/ ; terraform init'
                sh 'pwd;cd terraform/ ; terraform plan'
                //sh "pwd;cd terraform/ ; terraform plan -out tfplan"
                //sh 'pwd;cd terraform/ ; terraform show -no-color tfplan > tfplan.txt'
            }
        }

        stage('Infra-Apply') {
            when { expression { return params.apply } }
                steps {
                    sh "pwd;cd terraform/ ; terraform apply --auto-approve"
                    echo "Infra-Apply - params.applyc: ${params.apply}"
                }
             
        }

        stage('Infra-Destroy') {
            when { expression { return params.destroy } }
                steps {
                    sh "pwd;cd terraform/ ; terraform destroy --auto-approve"
                    echo "Infra-Apply - params.applyc: ${params.apply}"
                }
                
        }
    }

}
