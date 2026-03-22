pipeline {
    environment{
        DOCKERHUB_CREDENTIALS=credentials('DockerhubCred')
    }
    agent any

    stages {
        stage('Git Pull') {
            steps {
                git branch: 'main', url: 'https://github.com/nishuverma788/E-billpay.git'
            }
        }
        stage('Maven Build') {
            steps {
                dir('./backend') {
                    sh '''
		    export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
		    export PATH=$JAVA_HOME/bin:$PATH
		    java -version
 		    mvn -version
                    mvn clean install
		    '''
                }
            }
        }
        stage('Build Docker Images') {
            steps {
                dir('./backend') {
                    sh 'docker build -t nishu839/ebill-backend:latest .'
                }
                dir('./frontend') {
                    sh 'docker build -t nishu839/ebill-frontend:latest .'
                }
            }
        }
        stage('Dockerhub Login') {
            steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}
        stage('Push Docker Images') {
            steps {
                script{
                    sh 'docker push nishu839/ebill-backend'
                    sh 'docker push nishu839/ebill-frontend'
                }
            }
        }
        stage('Ansible Pull & Deploy') {
            steps {
               ansiblePlaybook colorized: true, disableHostKeyChecking: true, installation: 'Ansible', inventory: 'deploy-docker/inventory', playbook: 'deploy-docker/ebill-deploy.yml'
            }
        }

    }
    post {
		always {
			sh 'docker logout'
		}
	}
}
