pipeline{
    agent any

    tools {
        maven 'localMaven'
    }
    stages{
        stage('build'){
            steps{
                bat 'mvn clean package'
            }
            post{
                success{
                    echo 'now archiving...'
                    archiveArtifacts artifacts: '**/*.war' 
                }
            }
        }
        stage('deploy-to-staging'){
            steps{
                build job: 'Deploy-to-stage'
            }
        }

        stage('deploy-to-production'){
            steps{
                timeout(time:5, unit: 'DAYS'){
                    input message: 'Approve PRODUCTION deployment?'
                }
                build job:'Deploy-to-prod'
            }
            post{
                success{
                    echo 'Code deployed to production'
                }
                failure{
                    echo 'Deployment failed.'
                }
            }
        }
    }
}