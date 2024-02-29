pipeline{
    agent any
    stages{

        stage('SonarAnalysis'){
            steps{
                echo "<---------------START SONARANALYSIS--------------->"
                sh 'mvn clean verify sonar:sonar \
                    -Dsonar.projectKey=devsec1 \
                    -Dsonar.organization=devsec1 \
                    -Dsonar.host.url=https://sonarcloud.io \
                    -Dsonar.token=996a5e21647e3dfc637be01880a9f9e681a2129e'

                echo "<---------------END SONARANALYSIS--------------->"
            }
        }

        stage('SCA-synk-Analysis'){
            steps{
                echo "<---------------START SCA-SNYK-ANALYSIS--------------->"
                withCredentials([string(credentialsId:  'SNYK_TOKEN', variable: 'SNYK_TOKEN')]){
                    sh 'mvn snyk:test -fn'

                echo "<---------------END SCA-SNYK-ANALYSIS--------------->"                
                }
            }
        }
    }
}
