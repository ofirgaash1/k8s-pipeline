pipeline {
  agent any

  environment {
    AWS_REGION = 'il-central-1'
    ECR_REPO = '314525640319.dkr.ecr.il-central-1.amazonaws.com/ofirgaash/web'
    IMAGE_TAG = "v${BUILD_NUMBER}"
    CHART_DIR = "charts/silence-web"
    NAMESPACE = "ofirgaash"
    HELM_RELEASE = "silence-web"
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build Docker Image') {
      steps {
        dir('app') {
          sh 'docker build -t $ECR_REPO:$IMAGE_TAG .'
        }
      }
    }

    stage('Push to ECR') {
      steps {
        script {
          sh """
            aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $ECR_REPO
            docker push $ECR_REPO:$IMAGE_TAG
          """
        }
      }
    }

    stage('Deploy with Helm') {
      steps {
        sh """
          helm list -A
          kubectl get all -n ofirgaash
          helm upgrade $HELM_RELEASE $CHART_DIR \
            --namespace $NAMESPACE \
            --set image.repository=$ECR_REPO \
            --set image.tag=$IMAGE_TAG \
            --set image.pullPolicy=Always
        """
      }
    }
  }

  post {
    success {
      echo '✅ Deployed successfully!'
    }
    failure {
      echo '❌ Deployment failed.'
    }
  }
}

