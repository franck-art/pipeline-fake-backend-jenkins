pipeline {

    agent any
    stages { 

      
        stage('Build') {

            steps {

             sh 'docker  build -t fake-backend-jenkins ./fake-backend'
            }
        }
        stage('Test') {
           
            steps {
                sh 'docker-compose up -d'
                sh ' sleep 10s'
	        sh 'if [ "$(curl -X GET http://172.31.65.182:80/health)" = "ok" ] ; then echo "test OK";exit 0;  else echo "test KO" ;exit 1; fi'
            }
        }
        stage('Tag') {

            
             withCredentials ([string ( credentialsId:  'dockerhub_key' , variable:  'DOCKER_PASSWORD' )])         
            steps {
             sh 'docker login -u franckjunior -p $DOCKER_PASSWORD'
            sh 'docker tag fake-backend-jenkins franckjunior/fake-backend-jenkins:pipeline_tag'
            }
        
        }

         stage('Push') {
           
            when { branch 'master' }
            steps {
            sh 'docker push franckjunior/fake-backend-jenkins:pipeline_tag'
            }
        }
       stage('Clean') {

            steps {
            sh 'docker-compose down '
            }
        }

        
    }
}

