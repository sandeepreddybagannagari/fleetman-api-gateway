pipeline {
   agent any

   environment {
     // You must set the following environment variables
     // ORGANIZATION_NAME
     // YOUR_DOCKERHUB_USERNAME (it doesn't matter if you don't have one)

      SERVICE_NAME = "fleetman-api-gateway"
     // REPOSITORY_TAG="${YOUR_DOCKERHUB_USERNAME}/${ORGANIZATION_NAME}-${SERVICE_NAME}:${BUILD_ID}"
   }

   stages {
      stage('Preparation') {
         steps {
            cleanWs()
            git 'https://github.com/sandeepreddybagannagari/fleetman-api-gateway'
         }
      }
      stage('code analysis') {
         steps{
            script {
            withSonarQubeEnv('My SonarQube Server', envOnly: true) {
             // This expands the evironment variables SONAR_CONFIG_NAME, SONAR_HOST_URL, SONAR_AUTH_TOKEN that can be used by any script.
            println ${env.SONAR_HOST_URL} 
            }
           }
         }
      }
      stage('Build') {
         steps {
            sh '''mvn clean package'''
         }
      }

      stage('Build and Push Image') {
         steps {
           sh 'docker image build -t ${REPOSITORY_TAG} .'
         }
      }

      stage('Deploy to Cluster') {
          steps {
                    sh 'envsubst < ${WORKSPACE}/deploy.yaml | kubectl apply -f -'
          }
      }
   }
}
