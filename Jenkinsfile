pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deploy to Staging'){
            steps {
                build job: 'maven-project-deploy-staging'
            }
        }
        stage('Deploy to Production'){
            steps{
                timeout(time: 5, unit: 'DAYS'){
                    input message: 'Approve PRODUCTION deployment?'
                }

                build job: 'maven-project-deploy-prod'
            }
            post{
                success{
                    echo 'Successfully deployed to production'
                }
                failure{
                    echo 'Failed to deploy to production'
                }
            }
        }
    }
}
