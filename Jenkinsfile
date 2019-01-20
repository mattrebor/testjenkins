pipeline {
    agent any
    tools {
        maven 'Maven 3.6.0'
    }
    stages {
        stage('Build') { 
            steps {
                sh 'env' 
                sh 'mvn -B clean package'
            }
        }
        
        stage('Deploy') {
            steps {
               sh 'mvn tomcat7:deploy-only -Dmaven.tomcat.update=true'
            }
        }

    }
    post {
            always {

                emailext to: "abc@faygee.com", body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}",
                recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']],
                subject: "Jenkins Build : Job ${env.JOB_NAME} - ${env.TAG_NAME} - ${currentBuild.currentResult}"

            }
    }
}