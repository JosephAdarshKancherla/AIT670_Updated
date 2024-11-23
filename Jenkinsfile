pipeline{
    agent any
    environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub')
	}
  stages{
    stage('Build') {
      steps {
	sh 'rm -rf *.war'
        sh 'jar -cvf target/static-website.war -C "src/main/webapp" .'     
        sh 'docker build -t josephadarsh/ait670-app:v1.0 .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW |docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
       }
    }
    stage("Push image to docker hub"){
      steps {
        sh 'docker push josephadarsh/myapp:v1.0'
      }
    }
        stage("deploying on k8")
	{
		steps{
			sh 'kubectl set image deployment/ait670-deployment-deployment container-0=josephadarsh/myapp:v1.0 -n default'
			sh 'kubectl rollout restart deploy ait670-deployment-deployment -n default'
		}
	} 
  }
 
  post {
	  always {
			sh 'docker logout'
		}
	}    
}
