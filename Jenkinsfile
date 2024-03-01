pipeline {
    agent any
    stages {
        stage('Integrate Jenkins server with eks dev Cluster') {
            steps {
                script{
                    withAWS(credentials: 'aws-credentials', region: 'us-east-1'){
                    echo "<---------------Integrating eks cluster--------------->"
                    sh 'aws eks update-kubeconfig --name dev --region us-east-1'
                    echo "<---------------Integrated eks cluster--------------->"
                    }
                }
            }
        }
    }
}
