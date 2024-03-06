pipeline{
    agent any

    stages{
        stage('DAST-scan-using-OWASP-ZAP'){
            steps{
                script{

                    withAWS(credentials: 'aws-credentials', region: 'us-east-1'){
                        sh 'aws eks update-kubeconfig --name dev --region us-east-1'
                    }

                    withKubeConfig([credentialsId: 'kubelogin']){
                        sh('zap.sh -cmd -quickurl http://$(kubectl get services/buggy-app -n devsecops -o json | jq -r ".status.loadBalancer.ingress[] | .hostname") -quickprogress -quickout ${WORKSPACE}/zap_report.html')
                        archiveArtifacts artifacts: 'zap_report.html'
                    }
                }
                
            }
        }

    }
}

