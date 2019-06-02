pipeline {
    agent any
     tools {
       maven 'localMaven'
    }

    parameters {
         string(name: 'tomcat', defaultValue: 'localhost', description: 'Tomcat Server')
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
                        sh "whoami"
                        sh "cp -p **/target/*.war /opt/tomcat-staging/webapps"
                    }

                }

                stage ("Deploy to Production"){
                    steps {
                        sh "cp -p **/target/*.war /opt/tomcat-prod/webapps"
                    }
                }
            }
        }
    }
}
