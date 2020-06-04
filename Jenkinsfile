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
    }
}