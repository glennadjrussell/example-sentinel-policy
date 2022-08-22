pipeline {
    agent any
    stages {
        stage('Checkout') {
            checkout scm
        }

	stage('Run terraform init') {
	    steps {
	        sh "terraform init -input=false"
	    }
	}

	stage('Run terraform plan') {
	    steps {
	        sh "terraform plan -input=false -out tfplan"
	    }
	}

	stage('Run terraform apply') {
	    steps {
	        sh "terraform apply -input=false tfplan"
	    }
	}

	stage('Run static sentinel tests') {
	    steps {
	        sh "echo some sentinel tests"
	    }
	}

	stage('Configure TfE workspace') {
	    steps {
		def response = httpRequest 'https://app.terraform.io/api/v2/organizations/metasast/workspaces'
		println("Status: "+response.status)
		println("Content: "+response.content)
	    }
	}
    }
} 
