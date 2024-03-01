pipeline {
    agent any
    stages {
        stage('Create-K8s-Cluster') {
            steps {
                echo "<---------------STARTED K8S-CLUSTER-CREATING--------------->"
                // Use 'withCredentials' to provide AWS credentials to 'sh' step
                // Make sure 'aws-credentials' is the ID of your Jenkins credentials containing AWS access key and secret key
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws-credentials', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                    sh '''
                        # Set AWS CLI environment variables for AWS SDK to use
                        export AWS_ACCESS_KEY_ID="$AWS_ACCESS_KEY_ID"
                        export AWS_SECRET_ACCESS_KEY="$AWS_SECRET_ACCESS_KEY"
                        
                        # Execute eksctl command to create the EKS cluster
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
                }
                echo "<---------------ENDED K8S-CLUSTER-CREATING--------------->"
            }
        }
    }
}
