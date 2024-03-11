pipeline{
    agent any
    // tools{
    //     maven 'Maven_123'
    // }
    stages{

        stage('SonarAnalysis'){
            steps{
                sh 'mvn clean verify sonar:sonar \
                    -Dsonar.projectKey=devsec1 \
                    -Dsonar.organization=devsec1 \
                    -Dsonar.host.url=https://sonarcloud.io \
                    -Dsonar.token=996a5e21647e3dfc637be01880a9f9e681a2129e'
            }
        }
        
    }
}
