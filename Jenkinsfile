pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=devsecops321 -Dsonar.organization=devsecops321 -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=57aa2e966e0e3723dfb2dd3e16ccd227ca988f18'
			}
        } 
  }
}
