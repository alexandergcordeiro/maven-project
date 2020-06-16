pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '3.133.87.216', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.191.127.74', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
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

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        bat "winscp -i /dev/security/aws/aws-tomcat-public-key.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                    }
                }
 
                stage ("Deploy to Production"){
                    steps {
                        bat "winscp -i /dev/security/aws/aws-tomcat-public-key.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }
    }
}