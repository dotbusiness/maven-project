pipeline {
    agent any

    parameters {
         string(name: 'tomcat-staging', defaultValue: 'http://localhost:9090', description: 'Staging Server')
         string(name: 'tomcat-prod', defaultValue: 'http://localhost:9091', description: 'Production Server')
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
                        sh "cp**/target/*.war /home/chiix/Downloads/tomcat/tomcat-staging/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "cp**/target/*.war /home/chiix/Downloads/tomcat/tomcat-prod/webapps"
                    }
                }
            }
        }
    }
}