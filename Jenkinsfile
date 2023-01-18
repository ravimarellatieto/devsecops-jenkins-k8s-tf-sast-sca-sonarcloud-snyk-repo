pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
			sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=devsecopsbuggywebapp -Dsonar.organization=devsecopsbuggywebapp -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=3258eaf7218b8424dd5803fc9d013846c48f88f0'

			}
    }

	stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }
    stage('Build'){
      steps{
        withDockerRegistry([credentialsId: "dockerlogin",url:""]){
          script{
            app = docker.build("dbw")
          }
        }
      }
    }
    
    stage('Push to aws'){
      steps{
        script{
          docker.withRegistry('https://306387808166.dkr.ecr.us-west-2.amazonaws.com/dbw', 'ecr:us-west-2:aws-credentials'){
            app.push("latest")
          }
        }
      }
    }
  }
}
