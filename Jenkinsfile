pipeline{
    agent any
    // tools{
    //     maven 'Maven_123'
    // }
    stages{

        stage('SCA-snyk-Analysis'){
            steps{
                echo "<---------------START SCA-SNYK-ANALYSIS--------------->"
                withCredentials([string(credentialsId:  'SNYK_TOKEN', variable: 'SNYK_TOKEN')]){
                    sh 'mvn snyk:test -fn'

                echo "<---------------END SCA-SNYK-ANALYSIS--------------->"                
                }
            }
        }
        
    }
}
