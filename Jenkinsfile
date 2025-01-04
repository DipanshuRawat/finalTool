pipeline {

    parameters {
        booleanParam(name: 'autoApprove', defaultValue: false, description: 'Automatically run apply without manual approval?')
        choice(name: 'action', choices: ['apply', 'destroy'], description: 'Choose between Apply or Destroy')
    } 
    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        AWS_DEFAULT_REGION    = 'ap-south-1'
    }

    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/DipanshuRawat/finalTool.git', branch: 'main'
            }
        }

        stage('Plan') {
            steps {
                sh 'pwd; cd Terraform/ ; terraform init'
                sh 'pwd; cd Terraform/ ; terraform validate'
                sh 'pwd; cd Terraform/ ; terraform plan'
            }
        }

        stage('Apply or Destroy') {
            steps {
                script {
                    if (params.action == 'apply') {
                        if (params.autoApprove) {
                            sh 'pwd; cd Terraform/ ; terraform apply --auto-approve'
                        } else {
                            input message: 'Do you want to proceed with Apply?', ok: 'Yes'
                            sh 'pwd; cd Terraform/ ; terraform apply --auto-approve'
                        }
                    } else if (params.action == 'destroy') {
                        input message: 'Do you want to proceed with Destroy?', ok: 'Yes'
                        sh 'pwd; cd Terraform/ ; terraform destroy --auto-approve'
                    }
                }
            }
        }
    }
}
