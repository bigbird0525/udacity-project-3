pipeline {
  agent any
  stages {
    stage('AWS Credentials') {
      steps {
        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aravn', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
          sh """
                mkdir -p ~/.aws
                echo "[default]" > ~/.aws/credentials
                echo "[default]" > ~/.boto
                echo "aws_access_key_id = ${AWS_ACCESS_KEY_ID}" > ~/.boto
                echo "aws_secret_access_key = ${AWS_SECRET_ACCESS_KEY}" > ~/.boto
                echo "aws_access_key_id = ${AWS_ACCESS_KEY_ID}" > ~/.aws/credentials
                echo "aws_secret_access_key = ${AWS_SECRET_ACCESS_KEY}" > ~/.aws/credentials
          """
        }
      }
    }
    stage('Lint HTML') {
        steps {
          sh 'tidy -q -e *.html'
        }
    stage('Upload to AWS') {
      steps {
        withAWS(region:'us-east-1',credentials:'aravn') {
          s3Upload(pathStyleAccessEnabled:true, payloadSigningEnabled: true, file:'index.html', bucket:'aravn-udacity-projects')
          }
        }
      }
    }
  }
}