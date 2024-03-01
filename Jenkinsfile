pipeline{
    agent any 
    
    stages{

        

        //To create a cluster.
        stage('Create k8s cluster'){
            steps{
                withAWS(credentials: 'aws-credentials', region: 'us-east-1'){
                    echo "<---------------STARTED CREATING K8S CLUSTER--------------->"
                    sh 'aws eks update-kubeconfig --name dev --region us-east-1' //optional
                    echo "<---------------ENDED CREATING K8S CLUSTER--------------->"
          
                }
            }
        }

        //To create deployment for deploy buggy-app on k8s.
        stage('Deploy buggy-app on k8s'){
            steps{
                withkubeConfig([credentialsId: 'kubeconfig-file']){
                    echo "<---------------STARTED CREATING NAMESPACE & DEPLOYMENT ON K8S CLUSTER--------------->"
                    sh 'kubectl create ns devsecops'
                    sh 'kubectl delete all --all -n devsecops' //optional (To delete all resources inside the namespace)
                    echo "<---------------ENDED CREATING NAMESPACE & DEPLOYMENT K8S CLUSTER--------------->"
                    }
            }
        }

    }
}
