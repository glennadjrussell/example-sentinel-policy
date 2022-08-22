pipeline {
    agent any
    stages {
        stage('Checkout') {
	    steps {
                checkout scm
	    }
        }

	stage('Run terraform init') {
	    steps {
	        sh "echo terraform init -input=false"
	    }
	}

	stage('Run terraform plan') {
	    steps {
	        sh "echo terraform plan -input=false -out tfplan"
	    }
	}

	stage('Run terraform apply') {
	    steps {
	        sh "echo terraform apply -input=false tfplan"
	    }
	}

	stage('Run static sentinel tests') {
	    steps {
	        sh "echo some sentinel tests"
	    }
	}

	stage('Configure TfE workspace') {
	    steps {
	    	script {
		    withCredentials([string(credentialsId: 'TF_CLOUD_TOKEN', variable: 'auth')]) {
		        def response = httpRequest url: 'https://app.terraform.io/api/v2/organizations/metasast/workspaces', customHeaders:[[name:'Authorization', value:"Basic ${auth}"]]
		        println("Status: "+response.status)
		        println("Content: "+response.content)
		    }
		}
	    }
	}
    }
} 
