pipeline {
    agent any
    tools {
        maven 'Maven 3.6.0'
    }
    stages {
        stage('Build') { 
            steps {

                bat 'set'
                bat 'mvn -B clean package'
            }
        }
        
        stage('Deploy') {
            steps {
               bat 'mvn tomcat7:deploy-only'
            }
        }

        stage('Validate') {
            steps {
                sleep "${SLEEP_INTERVAL}"

                httpRequest consoleLogResponseBody: true,
                url: "${VALIDATION_URL}",
                validResponseCodes: '200',
                validResponseContent: "Deployed Tag: ${env.TAG_NAME}"
            }
        }

    }
    post {
            always {

                emailext to: "jenkins", body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\nDuration: ${currentBuild.duration} ms",
                recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']],
                subject: "Jenkins Build : ${currentBuild.currentResult} ${env.JOB_NAME}"

            }
    }
}