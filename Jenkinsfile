pipeline{
    agent any
    stages{

        stage('SonarAnalysis'){
            steps{
                script{
                    echo "<---------------STARTED SONARANALYSIS--------------->"
                    sh 'mvn clean verify sonar:sonar \
                    -Dsonar.projectKey=devsec1 \
                    -Dsonar.organization=devsec1 \
                    -Dsonar.host.url=https://sonarcloud.io \
                    -Dsonar.token=996a5e21647e3dfc637be01880a9f9e681a2129e'
                    echo "<---------------ENDED SONARANALYSIS--------------->"
                }
            }
        }

        stage('SCA-snyk-Analysis'){
            steps{
                script{
                    withCredentials([string(credentialsId:  'SNYK_TOKEN', variable: 'SNYK_TOKEN')]){
                        echo "<---------------STARTED SCA-SNYK-ANALYSIS--------------->"
                        sh 'mvn snyk:test -fn'
                        echo "<---------------ENDED SCA-SNYK-ANALYSIS--------------->"  
                    }        
                }
            }
        }

        stage('Build-Docker-Image'){
            steps{ 
                script{
                    withDockerRegistry([credentialsId: "docker-login", url: ""]){
                        echo "<---------------STARTED Build-Docker-Image--------------->"
                        app = docker.build("buggy-app")
                        echo "<---------------ENDED Build-Docker-Image--------------->"
                    }    
                }
            }
        }

        stage('Push-Docker-Image-to-ECR'){
            steps{
                script{
                    docker.withRegistry('https://533981927257.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:aws-credentials'){
                        "<---------------STARTED Push-Docker-Image-to-ECR--------------->"
                        app.push("v1")
                        "<---------------ENDED Push-Docker-Image-to-ECR--------------->"
                    }
                }
            }
        }

        //To create a cluster.
        stage('Create k8s cluster'){
            steps{
                withAWS(credentials: 'aws-credentials', region: 'us-east-1'){
                    echo "<---------------STARTED CREATING K8S CLUSTER--------------->"
                    sh 'eksctl create cluster --name dev --region us-east-1 --zones us-east-1d --nodegroup-name nodes-dev --node-type t3.medium --nodes 2 --nodes-min 1 --nodes-max 3 --managed'
                    sh 'aws eks update-kubeconfig --name dev --region us-east-1' //optional
                    echo "<---------------ENDED CREATING K8S CLUSTER--------------->"
          
                }
            }
        }

        //To create deployment for deploy buggy-app on k8s.
        stage('Deploy buggy-app on k8s'){
            steps{
                echo "<---------------STARTED CREATING NAMESPACE & DEPLOYMENT ON K8S CLUSTER--------------->"
                sh 'kubectl create ns devsecops'
                sh 'kubectl delete all --all -n devsecops' //optional (To delete all resources inside the namespace)
                sh 'kubectl create deployment devsec -n devsecops'
                echo "<---------------ENDED CREATING NAMESPACE & DEPLOYMENT K8S CLUSTER--------------->"
            }
        }

    }
}
