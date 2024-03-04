pipeline{
    agent any 

    stages{

        stage('Deploy buggy-app on k8s'){
            steps{
                script{
                    withAWS(credentials: 'aws-credentials', region: 'us-east-1'){
                        sh 'aws eks update-kubeconfig --name dev --region us-east-1'
                        sh 'sudo /home/ubuntu/bin/kubectl create ns devsecops'
                        sh 'sudo /home/ubuntu/bin/kubectl delete all --all -n devsecops' //optional (To delete all resources inside the namespace)
                        //sh 'kubectl create deployment devsec -n devsecops'
                    }
                }       
            }
        }

    }
}
