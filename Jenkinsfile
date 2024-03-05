pipeline{
    agent any

    stages{

        stage('Deploy buggy-app on k8s'){
            steps{
                withKubeConfig([credentialsId: 'kubelogin']){
                    echo "<---------------STARTED CREATING NAMESPACE & DEPLOYMENT K8S CLUSTER--------------->"
                    sh 'kubectl create ns devsecops'
                    sh 'kubectl delete all --all -n devsecops' //optional (To delete all resources inside the namespace)
                    sh 'kubectl create deployment devsec -n devsecops'
                    echo "<---------------ENDED CREATING NAMESPACE & DEPLOYMENT K8S CLUSTER--------------->"
                }
            }
        }

    }
}

