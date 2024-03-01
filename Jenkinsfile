pipeline{
    agent any
    stages{
        stage('Create-K8s-Cluster'){
            steps{
                script{
                    echo "<---------------STARTED K8S-CLUSTER-CREATING--------------->"
                    withAWS(credentials: 'aws-credentials', region: 'us-east-1'){
                        sh 'eksctl create cluster --name dev --region us-east-1 --zones us-east-1a, us-east-1d --nodegroup-name nodes-dev --node-type t3.medium --nodes 2 --nodes-min 1 --nodes-max 3 --managed'
                    }
                    echo "<---------------ENDED K8S-CLUSTER-CREATING--------------->"
                }
            }
        }
    }
}
