pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '54.211.129.19', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '3.87.231.183', description: 'Production Server')
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
                        sh "scp -i /home/jenkins/coditasadmin/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/usr/share/apache-tomcat-7.0.94/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/jenkins/coditasadmin/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/usr/share/apache-tomcat-7.0.94tomcat/webapps"
                    }
                }
            }
        }
    }
}