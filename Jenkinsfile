pipeline{
    agent any
    // tools{
    //     maven 'Maven_'
    // }
    stages{
        stage('SonarAnalysis'){
            steps{
                sh 'mvn clean verify sonar:sonar -Dsonar.projectkey=devsecops321 -Dsonar.organization=devsecops321 -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=7b28c0514caefe8081be6197282a3ac90f388fa1'
            }
        }
    }
}
