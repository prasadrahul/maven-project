pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '/home/prasad/JenkinsDemo/dev_tomcat', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '/home/prasad/JenkinsDemo/prod_tomcat', description: 'Production Server')
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
                            sh "cp **/target/*.war ${params.tomcat_dev}/webapps"
                        }
                    }

                    stage ("Deploy to Production"){
                        steps {
                            sh "cp **/target/*.war ${params.tomcat_prod}/webapps"
                        }
                    }
                }
            }
        }
}
