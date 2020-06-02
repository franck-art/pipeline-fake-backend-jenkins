pipeline {

    agent none 

    stages { 

      
        stage('Build') {

         agent {
    dockerfile {
          filename "fake-backend/Dockerfile"
        }
     }

            steps {

             sh 'docker-build -t fake-backend-jenkins ./fake-backend'
            }
        }
        stage('Test') {
           
            steps {
                sh 'docker-compose up -d'
				sh 'if [ "$(curl -X GET http://172.31.65.182:80/health)" = "ok" ] ; then echo "test OK";exit 0;  else echo "test KO" ;exit 1; fi'
            }
        }
        stage('Clean') {
           
            steps {
            sh 'docker-compose down -d'
            }
        }
        stage('Tag') {

            
           
            steps {
             sh 'docker login -u $DOCKER_LOGIN -p $DOCKER_PASSWORD'
            sh 'docker tag fake-backend-jenkins $DOCKER_LOGIN/fake-backend-jenkins:pipeline_tag'
            }
        }
         stage('Push') {
           
            when { branch 'master' }
            steps {
            sh 'docker push $DOCKER_LOGIN/fake-backend-jenkins:pipeline_tag'
            }
        }
    }
}

