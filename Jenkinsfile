pipeline {
	    agent any
	
	    parameters {
	         string(name: 'tomcat_dev', defaultValue: '172.31.27.84', description: 'Staging Server')
	         string(name: 'tomcat_prod', defaultValue: '172.31.18.229', description: 'Production Server')
	    }
	
	    triggers {
	         pollSCM('* * * * *')
	     }
	
	stages{
	        stage('Build'){
	            steps {
	                sh 'mvn clean package'
	            }
	            post {
	                success {
	                    echo 'Now Archiving...'
	                    archiveArtifacts artifacts: '**/target/*.war'
	                }
	            }
	        }
	
	        stage ('Deployments'){
	            parallel{
	                stage ('Deploy to QA'){
	                    steps {
	                        sh "scp -p -r /var/lib/jenkins/workspace/Vpofile-Pipeline/target/vprofile-v1.war devops@${params.tomcat_dev}:/usr/local/apache-tomcat-8.5.50/webapps"
	                    }
	                }
	
	                stage ("Deploy to Production"){
	                    steps {
	                        sh "scp -p -r /var/lib/jenkins/workspace/Vpofile-Pipeline/target/vprofile-v1.war devops@${params.tomcat_prod}:/usr/local/apache-tomcat-8.5.50/webapps"
	                    }
	                }
	            }
	        }
	    }
	}
