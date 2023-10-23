pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=asgbuggywebapp -Dsonar.organization=asgbuggywebapp -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=932558e169d66a8f1d1adf470b908a46156f5844'
			}
    }

	stage('RunSnykAnalysis') {
  	   steps {
               echo 'Scanning with Snyke. . . . .'
	       snykSecurity(
	         snykInstallation: 'orenSnyk',
	         snykTokenId: 'SNYK_TOKEN',
	         failOnError: 'false',
	         failOnIssues: 'false',
		 )
	      }
	  }
    

	stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app =  docker.build("orenr")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://422049563027.dkr.ecr.eu-central-1.amazonaws.com/orenr', 'ecr:eu-central-1:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}
	    
  }
}
