pipeline {
    agent any

     tools {
        maven 'localMaven'
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deploy to Staging'){
            steps {
                build job: 'deploy-to-staging'
            }
        }

        stage ('Deploy to Production') {
            steps{
                timeout(time:5, unit:'DAYS'){
                    input message : 'Aprove PRODUCTION Deployment?'
                }

                            build job : 'deploy-to-prod'
            }                    
            post{
                success {
                    echo 'Code deplyed to production.'
                }

                failure {
                    echo 'Deployement failed.'
                }
            }
            
        }

    }
}