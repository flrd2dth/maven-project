pipeline {
    agent any

    parameters {
        string(name: 'tomcat_dev', defaultValue: 'apache-tomcat-8.5.28-staging', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: 'apache-tomcat-8.5.28-prod', description: 'Production Server')
    }

    triggers {
        pollSCM('* * * * *')
    }

    tools {
        maven 'Default'
    }

    stages {
        stage('Build') {
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

        stage ('Deployments') {
            parallel {
                stage ('Deploy to Staging') {
                    steps {
                        sh "cp **/target/*.war /Users/strobins/Documents/${params.tomcat_dev}/webapps"
                    }
                }

                stage ("Deploy to Production") {
                    steps {
                        sh "cp **/target/*.war /Users/strobins/Documents/${params.tomcat_prod}/webapps"
                    }
                }
            }
        }
    }
}
