pipeline {
    agent {label 'bast'}
	

    environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub')
	}
    stages {

      stage('Docker Login') {
            steps {
              sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }


      stage('preparation'){
        steps{
            git branch: 'rds_redis', url: 'https://github.com/mahmoud254/jenkins_nodejs_example'
        }
      }

  

      stage('Build & push Dockerfile (CI)') {
            steps {
		     
		   
              sh """
                docker build . -f dockerfile -t 0xze/jenkins_nodejs
                docker push 0xze/jenkins_nodejs
              """
	    }
      }

      stage('Deploy the App (CD)') {
            steps {
                  sh """
			docker run -d --network host -p 3000:3000 -e RDS_HOSTNAME=myrdsinstance.cxogyruk5cvr.us-east-1.rds.amazonaws.com -e RDS_USERNAME=myrdsuser -e RDS_PASSWORD=myrdspassword -e RDS_PORT=3306 -e REDIS_HOSTNAME=cluster-example.ptqoxt.0001.use1.cache.amazonaws.com -e REDIS_PORT=6379  0xze/jenkins_nodejs:latest
                  """
		}
	    }
                
      }
}
