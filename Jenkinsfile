pipeline {
    agent {label 'nlp-slave'}

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
            git branch: 'rds_redis', url: 'https://github.com/0xZe/jenkins_nodejs_example'
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
			docker run -d --network host -p 3000:3000 -e RDS_HOSTNAME=database-2.cxogyruk5cvr.us-east-1.rds.amazonaws.com -e RDS_USERNAME=admin -e RDS_PASSWORD=123456789 -e RDS_PORT=3306 -e REDIS_HOSTNAME=edis.ptqoxt.clustercfg.use1.cache.amazonaws.com -e REDIS_PORT=6379  0xze/jenkins_nodejs:latest
                  """
		}
	    }
                
      }
}