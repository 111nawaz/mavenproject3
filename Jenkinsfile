pipeline {
	agent any
	environment {
    	PATH = "/opt/apache-maven-3.9.5/bin:$PATH"
	}
	stages {
    	stage("Clone Code") {
        	steps {
           	git branch: "main", url: "https://github.com/nnsnarasimha/maven.git"
        	}
    	}
    	stage("Build Code") {
        	steps {
            	sh "mvn clean install"
        	}
    	}
    	stage("S3 Upload Build") {
        	steps {
            	 sh "aws s3 cp /var/lib/jenkins/workspace/ s3://mpocket8 --recursive"
        	}
    	}
    	stage("Deploy DEV") {
        	steps {
            	sshagent(['deploy_user']) {
                	sh "scp -o StrictHostKeyChecking=no webapp/target/webapp.war ec2-user@54.162.220.75:/opt/apache-tomcat-9.0.82/webapps"
            	}
        	}    
    	}
 	}
}
