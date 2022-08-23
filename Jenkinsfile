pipeline {
    parameters {
        booleanParam(name: 'workspace_exists', defaultValue: true)
    }

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

	stage('Create/update TfE workspace') {
	    when {
	        expression {
		    return params.workspace_exists
		}
	    }

	    steps {
	    	script {
		    def bdy = """
		    {
                        "data": {
                            "attributes": {
                                "name": "workspace-creation-test-1",
                                "resource-count": 0,
                                "updated-at": "2017-11-29T19:18:09.976Z"
                            },
                            "type": "workspaces"
                        }
                    }
		    """

		    withCredentials([string(credentialsId: 'TF_CLOUD_TOKEN', variable: 'auth')]) {
		        def response = httpRequest contentType: 'application/vnd.api+json', url: 'https://app.terraform.io/api/v2/organizations/metasast/workspaces', customHeaders:[[name:'Authorization', value:"Bearer ${auth}"]], httpMode: 'POST', requestBody: bdy
		        println("Status: "+response.status)
		        println("Content: "+response.content)
		    }
		}
	    }
	}

	stage('Configure TfE workspace') {
	    steps {
	    	script {
		    withCredentials([string(credentialsId: 'TF_CLOUD_TOKEN', variable: 'auth')]) {
		        def response = httpRequest url: 'https://app.terraform.io/api/v2/organizations/metasast/workspaces', customHeaders:[[name:'Authorization', value:"Bearer ${auth}"]]
		        println("Status: "+response.status)
		        println("Content: "+response.content)
		    }
		}
	    }
	}
    }
} 
