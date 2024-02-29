pipeline{
    agent any
    tools{
        maven 'Maven_123'
    }
    stages{
        stage('SonarAnalysis'){
            steps{
                sh 'mvn clean verify sonar:sonar -Dsonar.projectkey=devsec1 -Dsonar.organization=devsec1 -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=6447df58bce1bd71ec18e663492f6d8cefda91eb'
            }
        }
    }
}
