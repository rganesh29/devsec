pipeline{
    agent any

    stages{

        stage('Deploy-buggy-app-on-k8s'){
            steps{
                withAWS(credentials: 'aws-credentials', region: 'us-east-1'){
                    echo "<---------------Deploying-application-on-k8s--------------->"
                    sh 'aws eks update-kubeconfig --name dev --region us-east-1'
                    //sh 'cp ~/.kube/config ~'
                    sh 'kubectl create ns devsecops'
                    sh 'kubectl delete all --all -n devsecops'
                    sh 'kubectl create -f deployment.yaml -n devsecops'
                    echo "<---------------Deployed-application-on-k8s--------------->"
                    // script{
                    //     sleep time: 2, unit: 'MINUTES'
                    //     //Running DAST-scan on deployed application on kubernetes.
                    //     echo "<---------------Started-ZAP-DAST-Scan--------------->"
                    //     def serviceInfo = sh(script: 'kubectl get svc -n devsecops', returnStdout: true).trim()
                    //     echo "kubectl get svc -o wide -n devsecops:"
                    //     echo serviceInfo
                    //     sh('zap.sh -cmd -quickurl http://$(kubectl get services/buggy-svc -n devsecops -o json | jq -r ".status.loadBalancer.ingress[] | .hostname") -quickprogress -quickout ${WORKSPACE}/zap_report.html')
                    //     archiveArtifacts artifacts: 'zap_report.html'
                    //     echo "<---------------Ended-ZAP-DAST-Scan--------------->"
                    // }
                    

                }
            }
        }

    }
}

