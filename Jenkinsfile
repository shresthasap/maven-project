
/* There are two ways to write pipeline as code
1. Groovy script
2. Declarative Pipeline - Below example is declarative Pipeline */
pipeline { 
    agent any
        tools {
        maven 'localMaven' //This was missing in original jenkinsfile
        }      
    stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deploy to Staging'){
            steps {
// This step trigger Deploy-to-staging job that we created on Jenkins         
                build job: 'Deploy-to-staging' 
                }
        }  
        
        stage ('Deploy to Production'){
            steps{
                timeout(time:5, unit:'DAYS'){
                    input message:'Approve PRODUCTION Deployment?'
                }

                build job: 'Deploy-to-Prod'
            }
            post {
                success {
                    echo 'Code deployed to Production.'
                }

                failure {
                    echo ' Deployment failed.'
                }
            }
        }
    }
}
