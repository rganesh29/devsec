pipeline{
    agent any
    stages{
        stage('Create-K8s-Cluster'){
            steps{
                echo "<---------------STARTED K8S-CLUSTER-CREATING--------------->"
                        sh '''
                        eksctl create cluster \
                            --name dev \
                            --region us-east-1 \
                            --nodegroup-name standard-workers \
                            --node-type t3.medium \
                            --nodes 2 \
                            --nodes-min 1 \
                            --nodes-max 3 \
                            --managed
                    '''
                    echo "<---------------ENDED K8S-CLUSTER-CREATING--------------->"
            }
        }
    }
}
