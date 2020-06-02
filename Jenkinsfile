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
    }
}