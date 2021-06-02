pipeline {
  agent any
  parameters {
    string(name: 'WORKSPACE', defaultValue: 'development', description:'setting up workspace for terraform')
  }
  environment {
    TF_HOME = tool('terraform-0.15.3')
    TF_IN_AUTOMATION = "true"
    PATH = "$TF_HOME:$PATH"
  }
  stages {
    stage('TerraformInit'){
        steps {
            sh "terraform init -input=false"
        }
    }
    stage('Terraform workspace') {
        steps {
          script {
                try {
                    sh "terraform workspace new ${params.WORKSPACE}"
                } catch (err) {
                    sh "terraform workspace select ${params.WORKSPACE}"
                }
          }
        }
    }
    stage('TerraformPlan'){
        steps {
            script {
                sh "terraform plan  -out terraform.tfplan;echo \$? > status"
                stash name: "terraform-plan", includes: "terraform.tfplan"
            }
        }
    }
    stage('TerraformApply'){
        steps {
            script{                    
                unstash "terraform-plan"
                sh "terraform apply terraform.tfplan"
            }
        }
    }
  }
  
}
