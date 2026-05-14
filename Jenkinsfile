pipeline{
  agent any
  environment{
    DOCKERIMAGE="bhumi3108/image10"
    DOCKERTAG="latest"
  }
  stages{
    stage('build'){
      steps{
        sh 'docker build -t $DOCKERIMAGE:$DOCKERTAG .'
      }
    }
    stage('login'){
      steps{
        withCredentials([usernamePassword(
          credentialsId:'dockerhub-credential',
          usernameVariable:'USER',
          passwordVariable:'PASS'
          )])
        {
          sh 'echo $PASS | docker login -u $USER --password-stdin'
        }
      }
    }
    stage('push image'){
      steps{
        sh 'docker push $DOCKERIMAGE:$DOCKERTAG'
      }
    }
    stage('deploy'){
      steps{
        sh '''
        docker stop my-app-container || true
        docker rm my-app-container || true
        docker run -d -p 5000:5000 --name my-app-container $DOCKERIMAGE:$DOCKERTAG
        '''
      }
    }
  }
}
