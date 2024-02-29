pipeline{
    agent any
    stages{

        stage('SonarAnalysis'){
            steps{
                echo "<---------------STARTED SONARANALYSIS--------------->"
                sh 'mvn clean verify sonar:sonar \
                    -Dsonar.projectKey=devsec1 \
                    -Dsonar.organization=devsec1 \
                    -Dsonar.host.url=https://sonarcloud.io \
                    -Dsonar.token=996a5e21647e3dfc637be01880a9f9e681a2129e'

                echo "<---------------ENDED SONARANALYSIS--------------->"
            }
        }

        stage('SCA-synk-Analysis'){
            steps{
                echo "<---------------STARTED SCA-SNYK-ANALYSIS--------------->"
                withCredentials([string(credentialsId:  'SNYK_TOKEN', variable: 'SNYK_TOKEN')]){
                    sh 'mvn snyk:test -fn'

                echo "<---------------ENDED SCA-SNYK-ANALYSIS--------------->"                
                }
            }
        }

        stage('Build-Docker-Image'){
            steps{
                echo "<---------------STARTED Build-Docker-Image--------------->"
                withDockerRegistry([string(credentialsId: "docker-login", url: "")]){
                    script{
                        app = docker.build("buggy-app")
                    }
                    echo "<---------------ENDED Build-Docker-Image--------------->"
                }
            }
        }

        stage('Push-Docker-Image-to-ECR'){
            steps{
                "<---------------STARTED Push-Docker-Image-to-ECR--------------->"
                docker.withRegistry('https://344680520302.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:aws-credentials'){
                    script{
                        app.push("v1")
                    }
                    "<---------------ENDED Push-Docker-Image-to-ECR--------------->"
                }
            }
        }
    }
}
