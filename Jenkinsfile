pipeline{
        agent {
                node {
                        label 'jenkins'
                }
        }
    stages{
        
        stage('Create k8s cluster'){
            steps{
                withAWS(credentials: 'aws-credentials', region: 'us-east-1'){
                    echo "<---------------STARTED CREATING K8S CLUSTER--------------->"
                    sh 'aws eks update-kubeconfig --name dev --region us-east-1' //optional
                    sh 'kubectl create ns devsecops'
                    echo "<---------------ENDED CREATING K8S CLUSTER--------------->"
          
                }
            }
        }

    }
}
