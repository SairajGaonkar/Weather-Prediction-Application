pipeline {
    agent any
    stages {
        stage('Installing dependencies') {
            steps {                 
                sh '''
       
                    sudo apt --assume-yes install maven
                    cd "ui-api-service"
                    mvn clean
                    mvn compile
                    mvn package
                    
                    '''
             }
        }
        stage('Building Docker image') {
            steps {
            sh '''
                sudo apt --assume-yes install docker.io
                sudo systemctl start docker
                sudo systemctl enable docker 
				python -m pip uninstall -y urllib3
				python -m pip install urllib3==1.22
                sudo docker-compose build --pull
				
            '''    
            }
        }
        stage('Pushing to Docker hub') {
            steps {
            sh '''
                sudo docker login --username=sgarandomerror --password=randomerror
                sudo docker-compose push
            '''    
            }
        }
		stage('SSH call to Kubernetes master - RabbitMQ') {
            steps {
            sh '''
				chmod 400 RandomError-api-key
				ssh -o StrictHostKeyChecking=no -i RandomError-api-key ubuntu@149.165.170.111 uptime
				ssh -i RandomError-api-key ubuntu@149.165.170.111 "sudo apt install gnupg2 pass && 
				sudo apt-get update &&
				sudo apt-get install -y kubectl &&
                sudo rm -rf RandomError &&
				git clone -b blue-deployment https://github.com/airavata-courses/RandomError.git &&
				cd ~/RandomError/
				sudo kubectl apply -f rabbitmq-config.yaml"
            '''    
            }
        }
		stage('SSH call to Kubernetes master - ui api') {
            steps {
            sh '''
				chmod 400 RandomError-api-key
				ssh -o StrictHostKeyChecking=no -i RandomError-api-key ubuntu@149.165.170.111 uptime
				ssh -i RandomError-api-key ubuntu@149.165.170.111 "sudo apt install gnupg2 pass && 
				sudo docker login --username=sgarandomerror --password=randomerror &&
				sudo docker pull sgarandomerror/ui-api:v0.2 &&
				sudo apt-get update &&
				sudo apt-get install -y kubectl &&
				sudo rm -rf RandomError &&
				git clone -b blue-deployment https://github.com/airavata-courses/RandomError.git &&
				cd ~/RandomError/ui-api-service &&
				sudo kubectl apply -f ui-api-config.yaml"

            '''    
            }
        }

		stage('SSH call to Kubernetes master - data retrieval') {
            steps {
            sh '''
				chmod 400 RandomError-api-key
				ssh -o StrictHostKeyChecking=no -i RandomError-api-key ubuntu@149.165.170.111 uptime
				ssh -i RandomError-api-key ubuntu@149.165.170.111 "sudo apt install gnupg2 pass && 
				sudo docker login --username=sgarandomerror --password=randomerror &&
				sudo docker pull sgarandomerror/dataretrieval:v0.2 &&
				sudo apt-get update &&
				sudo apt-get install -y kubectl &&
				sudo rm -rf RandomError &&
				git clone -b blue-deployment https://github.com/airavata-courses/RandomError.git &&
				cd ~/RandomError/dataretrieval-service &&
				sudo kubectl apply -f dataretrieval-config.yaml"

				#sudo kubectl run dataretrieval --image sgarandomerror/dataretrieval:v0.2"
            '''    
            }
        }
		stage('SSH call to Kubernetes master - store session') {
            steps {
            sh '''
				chmod 400 RandomError-api-key
				ssh -o StrictHostKeyChecking=no -i RandomError-api-key ubuntu@149.165.170.111 uptime
				ssh -i RandomError-api-key ubuntu@149.165.170.111 "sudo apt install gnupg2 pass && 
				sudo docker login --username=sgarandomerror --password=randomerror &&
				sudo docker pull sgarandomerror/sessionstore:v0.1 &&
				sudo apt-get update &&
				sudo apt-get install -y kubectl &&
				sudo rm -rf RandomError &&
				git clone -b blue-deployment https://github.com/airavata-courses/RandomError.git &&
				cd ~/RandomError/sessionmanagement-service/sessionStore &&
				sudo kubectl apply -f session-store-config.yaml"
            '''    
            }
        }
		stage('SSH call to Kubernetes master - retrieve session') {
            steps {
            sh '''
				chmod 400 RandomError-api-key
				ssh -o StrictHostKeyChecking=no -i RandomError-api-key ubuntu@149.165.170.111 uptime
				ssh -i RandomError-api-key ubuntu@149.165.170.111 "sudo apt install gnupg2 pass && 
				sudo docker login --username=sgarandomerror --password=randomerror &&
				sudo docker pull sgarandomerror/sessionretrieve:v0.1 &&
				sudo apt-get update &&
				sudo apt-get install -y kubectl &&
				sudo rm -rf RandomError &&
				git clone -b blue-deployment https://github.com/airavata-courses/RandomError.git &&
				cd ~/RandomError/sessionmanagement-service/sessionRetrieve &&
				sudo kubectl apply -f session-retrieve-config.yaml"
            '''    
            }
        }
		stage('SSH call to Kubernetes master - user management') {
            steps {
            sh '''
				chmod 400 RandomError-api-key
				ssh -o StrictHostKeyChecking=no -i RandomError-api-key ubuntu@149.165.170.111 uptime
				ssh -i RandomError-api-key ubuntu@149.165.170.111 "sudo apt install gnupg2 pass && 
				sudo docker login --username=sgarandomerror --password=randomerror &&
				sudo docker pull sgarandomerror/usermanagement:v0.1 &&
				sudo apt-get update &&
				sudo apt-get install -y kubectl &&
				sudo rm -rf RandomError &&
				git clone -b blue-deployment https://github.com/airavata-courses/RandomError.git &&
				cd ~/RandomError/usermanagement-service &&
				sudo kubectl apply -f usermanagement-config.yaml
				"
            '''    	
            }
        }
		stage('SSH call to Kubernetes master - data modeling') {
            steps {
            sh '''
				chmod 400 RandomError-api-key
				ssh -o StrictHostKeyChecking=no -i RandomError-api-key ubuntu@149.165.170.111 uptime
				ssh -i RandomError-api-key ubuntu@149.165.170.111 "sudo apt install gnupg2 pass && 
				sudo docker login --username=sgarandomerror --password=randomerror &&
				sudo docker pull sgarandomerror/datamodeling:v0.2 &&
				sudo apt-get update &&
				sudo apt-get install -y kubectl &&
				sudo rm -rf RandomError &&
				git clone -b blue-deployment https://github.com/airavata-courses/RandomError.git &&
				cd ~/RandomError/datamodeling-service &&
				sudo kubectl apply -f datamodeling-pv.yaml &&
				sudo kubectl apply -f datamodeling-config.yaml &&
                sudo kubectl apply -f datamodeling-service.yaml"
            '''    
            }
        }

    }
}