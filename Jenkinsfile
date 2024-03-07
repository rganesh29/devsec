pipeline{
    agent any

    stages{

        stage('SonarAnalysis'){
            steps{
                script{
                    echo "<---------------STARTED SONARANALYSIS--------------->"
                    sh 'mvn clean verify sonar:sonar \
                    -Dsonar.projectKey=devsec1 \
                    -Dsonar.organization=devsec1 \
                    -Dsonar.host.url=https://sonarcloud.io \
                    -Dsonar.token=996a5e21647e3dfc637be01880a9f9e681a2129e'
                    echo "<---------------ENDED SONARANALYSIS--------------->"
                }
            }
        }

        stage('SCA-snyk-Analysis'){
            steps{
                script{
                    withCredentials([string(credentialsId:  'SNYK_TOKEN', variable: 'SNYK_TOKEN')]){
                        echo "<---------------STARTED SCA-SNYK-ANALYSIS--------------->"
                        sh 'mvn snyk:test -fn'
                        echo "<---------------ENDED SCA-SNYK-ANALYSIS--------------->"  
                    }        
                }
            }
        }

        stage('Build-Docker-Image'){
            steps{ 
                script{
                    withDockerRegistry([credentialsId: "docker-login", url: ""]){
                        echo "<---------------STARTED Build-Docker-Image--------------->"
                        app = docker.build("buggy-app")
                        echo "<---------------ENDED Build-Docker-Image--------------->"
                    }    
                }
            }
        }

        stage('Push-Docker-Image-to-ECR'){
            steps{
                script{
                    docker.withRegistry('https://997372734429.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:aws-credentials'){
                        "<---------------STARTED Push-Docker-Image-to-ECR--------------->"
                        app.push("v1")
                        "<---------------ENDED Push-Docker-Image-to-ECR--------------->"
                    }
                }
            }
        }

        stage('Create k8s cluster'){
            steps{
                withAWS(credentials: 'aws-credentials', region: 'us-east-1'){
                    echo "<---------------STARTED CREATING K8S CLUSTER--------------->"
                    sh 'eksctl create cluster --name dev --region us-east-1 --zones us-east-1a,us-east-1d --nodegroup-name nodes-dev --node-type t3.medium --nodes 2 --nodes-min 1 --nodes-max 3 --managed'
                    //After k8s-cluster created Login to the server copy the .kube/config file and 
                    //Add that file in jenkins credentials(secret file).
                }
            }
        }

        // Optional 
        stage('Lets wait for few minutes to Create k8s Cluster'){
            steps{
                sh 'sleep 120; echo "<-------K8s-Cluster-Created------->"'
            }
        }

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
                    script{
                        sleep time: 2, unit: 'MINUTES'
                        //Running DAST-scan on deployed application on kubernetes.
                        echo "<---------------Started-ZAP-DAST-Scan--------------->"
                        def serviceInfo = sh(script: 'kubectl get svc -n devsecops', returnStdout: true).trim()
                        echo "kubectl get svc -o wide -n devsecops:"
                        echo serviceInfo
                        sh('zap.sh -cmd -quickurl http://$(kubectl get services/buggy-svc -n devsecops -o json | jq -r ".status.loadBalancer.ingress[] | .hostname") -quickprogress -quickout ${WORKSPACE}/zap_report.html')
                        archiveArtifacts artifacts: 'zap_report.html'
                        echo "<---------------Ended-ZAP-DAST-Scan--------------->"
                    }
                    

                }
            }
        }

    }
}

