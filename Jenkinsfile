pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: 'prasad@localhost:/home/prasad/JenkinsDemo/dev_tomcat', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'prasad@localhost:/home/prasad/JenkinsDemo/prod_tomcat', description: 'Production Server')
    }

    triggers {
         pollSCM('*/1 * * * *')
     }

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
                        echo 'Tick Tick Now Archiving...'
                        archiveArtifacts artifacts: '**/target/*.war'
                    }
                }
            }

            stage ('Deployments'){
                parallel{
                    stage ('Deploy to Staging'){
                        steps {
                            sh "scp **/target/*.war ${params.tomcat_dev}/webapps"
                        }
                    }

                    stage ("Deploy to Production"){
                        steps {
                            sh "scp **/target/*.war ${params.tomcat_prod}/webapps"
                        }
                    }
                }
            }
        }
}
