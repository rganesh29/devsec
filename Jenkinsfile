pipeline{
    agent any

    stages{
        
        stage('Create k8s cluster'){
            steps{
                withAWS(credentials: 'aws-credentials', region: 'us-east-1'){
                    echo "<---------------STARTED CREATING K8S CLUSTER--------------->"
                    //sh 'eksctl create cluster --name dev --region us-east-1 --zones us-east-1a,us-east-1d --nodegroup-name nodes-dev --node-type t3.medium --nodes 2 --nodes-min 1 --nodes-max 3 --managed'
                    sh 'aws eks update-kubeconfig --name dev --region us-east-1' //optional
                    //sh 'cp ./.kube/config /var/lib/jenkins/workspace/devsecops'
                    //sh 'kubectl create ns devsecops'
                    sh 'kubectl delete all --all -n devsecops'
                    sh 'kubectl create -f deployment.yaml -n devsecops'
                    script{
                        sleep time: 1, unit: 'MINUTES'
                        def serviceInfo = sh(script: 'kubectl get svc -n devsecops', returnStdout: true).trim()
                        echo "kubectl get svc -o wide -n devsecops:"
                        echo serviceInfo
                        sh('zap.sh -cmd -quickurl http://$(kubectl get services/buggy-svc -n devsecops -o json | jq -r ".status.loadBalancer.ingress[] | .hostname") -quickprogress -quickout ${WORKSPACE}/zap_report.html')
                        archiveArtifacts artifacts: 'zap_report.html'
                    }
                    echo "<---------------ENDED CREATING K8S CLUSTER--------------->"
                    
                }
            }
        }

    }
}

