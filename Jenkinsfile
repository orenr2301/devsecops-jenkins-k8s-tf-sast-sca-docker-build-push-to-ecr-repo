pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=orenr2301 -Dsonar.organization=orenr2301 -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=bb31b725e0926a4b0b0104af44445a23e4218dc6'
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
