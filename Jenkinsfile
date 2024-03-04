pipeline{
    agent any 

    stages{

        stage('Deploy buggy-app on k8s'){
            steps{
                script{
                    withAWS(credentials: 'aws-credentials', region: 'us-east-1'){
                    echo "<---------------STARTED CREATING NAMESPACE & DEPLOYMENT ON K8S CLUSTER--------------->"
                    sh 'aws eks update-kubeconfig --name dev --region us-east-1'
                    sh 'kubectl create ns devsecops'
                    }

                    // withKubeConfig([credentialsId: 'kubeconfig-file']){
                    // sh 'kubectl create ns devsecops'
                    // sh 'kubectl delete all --all -n devsecops' //optional (To delete all resources inside the namespace)
                    // sh 'kubectl create deployment devsec -n devsecops'
                    // echo "<---------------ENDED CREATING NAMESPACE & DEPLOYMENT K8S CLUSTER--------------->"
                    // }
                }       
            }
        }

    }
}
