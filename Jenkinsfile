pipeline {
     agent { label 'slave'}
        environment {
            WEBHOOK_URL="https://api.telegram.org/bot1491050411:AAHMArkFwHK_D1-PbxvbyMkzj-QkN1gzfHw/sendMessage"
            USERID="-400987297"
        }
    
    stages {
        stage('set up dependencies') {
            steps {
            sh 'npm install'
		}
        }
        stage('start app') {
            steps {
            sh 'nohup node index.js &'
		}
        }
        stage('unit test') {
            steps {
            sh 'npm test'
		}
        }
        stage('write result') {
            steps {
            junit 'test.xml'
		}
        }
       stage('build') {
            steps {
            sh 'docker build -t registry.gitlab.com/qminhh/test-01 .'
		}
       }
        stage('push') {
            steps {
            withDockerRegistry(credentialsId: 'jenkins1234', url: 'https://registry.gitlab.com') {
            sh 'docker push registry.gitlab.com/qminhh/test-01'
                    }
		        }
        }
    }
    post {
        success {
            sh """ 
                curl -X POST \
     -H 'Content-Type: application/json' \
     -d '{"chat_id": ${BUILD_LOG} "${USERID}", "text": " \ud83d\udc4d \ud83d\udc4d \ud83d\udc4d \nJobname: ${JOB_NAME} \nBuild number: ${BUILD_NUMBER} \nStatus: SUCCESS \nCommit: ${GIT_COMMIT} ", "disable_notification": true}' ${WEBHOOK_URL}
                """
        }
        }
    }
