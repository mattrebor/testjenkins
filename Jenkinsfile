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

        stage('Validaton') {
            steps {
                sleep 10

                httpRequest consoleLogResponseBody: true,
                url: "${VALIDATION_URL}",
                validResponseCodes: '200',
                validResponseContent: "Deployed Tag: ${env.TAG_NAME}"
            }
        }

    }
    post {
            always {

                emailext to: "jenkins", body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\nDuration: ${currentBuild.duration} ms\n\nChange Sets:\n${currentBuild.changeSets}",
                recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']],
                subject: "Jenkins Build : ${currentBuild.currentResult} ${env.JOB_NAME}"

            }
    }
}