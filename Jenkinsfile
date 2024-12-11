pipeline {

         agent { 

	        node {

	            label "built-in"
	 
                 }
                 
	           }

	 environment {

	 AWS_credentials = credentials("AWS_credentials_id")
         
	             }

	 stages {

	 stage("Checkout") {

	                   steps {
             
	                           git branch: 'master' , url: 'https://github.com/dhan1411/advance.git'
	                         }

	                    }
	 

	
	stage("Terraform_infra_creation") {
	     
                                        steps {
                                            
                                             dir('jenkins_advance') {
                                             
                                             script {
             
	                                            sh ' terraform init '
	                                            sh ' terraform plan '
	                                            sh ' terraform apply --auto-approve'

                                   env.PUBLIC_IP = sh(script: 'terraform output -raw ec2_public_ip', returnStdout: true).trim()
                                   env.HOSTNAME = sh(script: 'terraform output -raw db_endpoint', returnStdout: true).trim()
            
            
	                                                }   
	                                                
                                             }
                                                }  
	                                   }

    stage("Connect_db_instance") {

                                           steps {    
                                           withCredentials([
                                           usernamePassword(credentialsId: 'my-db-credentials', usernameVariable: 'DB_USERNAME',passwordVariable: 'DB_PASSWORD'),
                                           sshUserPrivateKey(credentialsId: 'my-ssh-key', keyFileVariable: 'SSH_KEY')
                                                           ]) {

                                                               sh ''' 
                                                                    ssh -o StrictHostKeyChecking=no -i ${SSH_KEY} ec2-user@${PUBLIC_IP} << EOF
                                                                         sudo mysql -h ${HOSTNAME} -u ${DB_USERNAME} -p${DB_PASSWORD} -e "show databases;"
EOF
'''

                                                               }
                                                    }
                                }	 
	 
	        }
	 }

