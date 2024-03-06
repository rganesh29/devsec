pipeline{
    agent any

    stages{

        stage('Create k8s cluster'){
            steps{
                withAWS(credentials: 'aws-credentials', region: 'us-east-1'){
                    echo "<---------------STARTED CREATING K8S CLUSTER--------------->"
                    //sh 'eksctl create cluster --name dev --region us-east-1 --zones us-east-1a,us-east-1d --nodegroup-name nodes-dev --node-type t3.medium --nodes 2 --nodes-min 1 --nodes-max 3 --managed'
                    sh 'aws eks update-kubeconfig --name dev --region us-east-1' //optional
                    //sh 'kubectl create ns devsecops'
                    sh 'kubectl delete all --all -n devsecops'
                    sh 'kubectl create deployment devsec -n devsecops'
                    echo "<---------------ENDED CREATING K8S CLUSTER--------------->"
          
                }
            }
        }


        // //To create deployment for deploy buggy-app on k8s.
        // stage('Deploy buggy-app on k8s'){
        //     steps{
        //         withKubeConfig([credentialsId: 'kubelogin']){
        //             echo "<---------------STARTED CREATING NAMESPACE & DEPLOYMENT K8S CLUSTER--------------->"
        //             sh 'kubectl create ns devsecops'
        //             sh 'kubectl delete all --all -n devsecops' //optional (To delete all resources inside the namespace)
        //             sh 'kubectl create deployment devsec -n devsecops'
        //             echo "<---------------ENDED CREATING NAMESPACE & DEPLOYMENT K8S CLUSTER--------------->"
        //         }
        //     }
        // }

    }
}

