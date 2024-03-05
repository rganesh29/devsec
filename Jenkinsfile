pipeline{
    agent any

    stages{
        steps{
            script{
                sh 'curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.28.5/2024-01-04/bin/linux/amd64/kubectl'
                sh 'chmod +x ./kubectl'
                // sh 'mkdir /var/lib/jenkins/bin'
                sh 'echo "export PATH=$HOME/bin:$PATH" >> ~/.bashrc'
'
            }
        }
    }
}

