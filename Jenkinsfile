pipeline {
    agent any

    parameters {
           /*parameters to supply in the deployment execution statement*/
           string(name: 'tomcat_stage', defaultValue: '52.87.178.24', description: 'Staging Server')
           string(name: 'tomcat_prod', defaultValue: '52.91.113.146', description: 'Production Server')    }

    triggers {
        /*poll the git rep every 1 minute*/
        pollSCM('* * * * *')
    }

    stages {
            stage('Build'){
                steps {
                    sh 'mvn clean package'
                }
                post {
                    success {
                        echo 'Now Archiving'
                        archiveArtifacts artifacts: '**/target/*.war'
                    }
                }
            }
            stage('Deployments'){
                parallel{
                    stage('Deploy to Staging'){
                        steps {
                             sh "scp -i /Users/sami/Desktop/ssh_aws_key/tomcat-staging.pem **/target/*.war ec2-user@${params.tomcat_stage}:/var/lib/tomcat7/webapps"
                        }
                    }
                    stage ("Deploy to Production") {
                        steps {
                             sh "scp -i /Users/sami/Desktop/ssh_aws_key/tomcat-prod.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"

                        }
                    }
                }
            }
    }
}