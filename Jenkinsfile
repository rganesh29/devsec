pipeline{
    agent any

    stages{

        stage('Install Kubectl'){
            steps{
                script{
                    sh 'curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.28.5/2024-01-04/bin/linux/amd64/kubectl'
                    sh 'chmod +x ./kubectl'
                    sh 'mkdir /var/lib/jenkins/bin'
                    sh 'sudo cp /var/lib/jenkins/kubectl /var/lib/jenkins/bin/'
                }
            }
        }


        //To create deployment for deploy buggy-app on k8s.
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

