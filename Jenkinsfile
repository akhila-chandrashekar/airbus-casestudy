pipeline {
  agent any
  stages {
    stage('Build and Deploy  to ECR') {
      parallel {
        stage('Calculation Service ') {
          agent any
          steps {
            dir(path: 'source/calculation-offer-service/CalculationServiceAPISolution') {
              sh '''pwd
'''
              sh 'docker build -t $CALCULATION_SERVICE_IMAGE:latest -t $CALCULATION_SERVICE_IMAGE:$BUILD_NUMBER .'
              sh 'docker tag $CALCULATION_SERVICE_IMAGE:latest $ECR_ID/$CALCULATION_SERVICE_IMAGE:latest'
              sh 'docker tag $CALCULATION_SERVICE_IMAGE:$BUILD_NUMBER $ECR_ID/$CALCULATION_SERVICE_IMAGE:$BUILD_NUMBER'
              sh 'docker login --username $ECR_CREDENTIALS_USR --password $ECR_CREDENTIALS_PSW $ECR_ID'
              sh 'docker image prune -f'
              sh 'docker push $ECR_ID/$CALCULATION_SERVICE_IMAGE:latest'
            }

          }
        }

        stage('Validation Response Daemon') {
          steps {
            dir(path: 'source/creditcard-identity-verification-response-daemon') {
              sh 'pwd'
              sh 'docker build -t $VALIDATION_RESPONSE_DAEMON_IMAGE:latest -t $VALIDATION_RESPONSE_DAEMON_IMAGE:$BUILD_NUMBER .'
              sh 'docker tag $VALIDATION_RESPONSE_DAEMON_IMAGE:latest $ECR_ID/$VALIDATION_RESPONSE_DAEMON_IMAGE:latest'
              sh 'docker login --username $ECR_CREDENTIALS_USR --password $ECR_CREDENTIALS_PSW $ECR_ID'
              sh 'docker image prune -f'
              sh 'docker push $ECR_ID/$VALIDATION_RESPONSE_DAEMON_IMAGE:latest'
            }

          }
        }

      }
    }

  }
  environment {
    ECR_ID = '142198642907.dkr.ecr.ap-northeast-1.amazonaws.com'
    CALCULATION_SERVICE_IMAGE = 'akhila-v2-casestudy-calculation-service'
    ECR_CREDENTIALS = credentials('ecr-credentials')
    VALIDATION_RESPONSE_DAEMON_IMAGE = 'akhila-v2-casestudy-creditcard-identity-verification-response-daemon'
    EMAIL_SERVICE_IMAGE = 'akhila-v2-casestudy-email-service'
    CREDITCARD_SERVICE_IMAGE = 'akhila-v2-casestudy-creditcard-service'
    IDENTITY_VERIFICATION_SERVICE_IMAGE = 'akhila-v2-casestudy-identity-verification-service'
  }
}