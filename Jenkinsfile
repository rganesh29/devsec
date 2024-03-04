pipeline{
    agent any 

    stages{

        


        //To create deployment for deploy buggy-app on k8s.
        stage('Deploy buggy-app on k8s'){
            steps{
                withKubeConfig([credentialsId: 'kubeconfig-file']){
                    echo "<---------------STARTED CREATING NAMESPACE & DEPLOYMENT K8S CLUSTER--------------->"
                    sh 'kubectl create ns devsecops'
                    sh 'kubectl delete all --all -n devsecops' //optional (To delete all resources inside the namespace)
                    //sh 'kubectl create deployment devsec -n devsecops'
                    echo "<---------------ENDED CREATING NAMESPACE & DEPLOYMENT K8S CLUSTER--------------->"
                }
            }
        }

    }
}
