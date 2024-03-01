pipeline{
    agent any
    stages{

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
                    docker.withRegistry('https://592640137521.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:aws-credentials'){
                        "<---------------STARTED Push-Docker-Image-to-ECR--------------->"
                        app.push("v1")
                        "<---------------ENDED Push-Docker-Image-to-ECR--------------->"
                    }
                }
            }
        }

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
