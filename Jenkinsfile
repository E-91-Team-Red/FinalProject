pipeline {
    agent any
    stages {
        stage("pull-updates-to-dev"){
            steps {
                sshagent (credentials: ['admin']) {
                    sh "ssh -o StrictHostKeyChecking=no e91user@3.91.19.106 'cd /home/e91user/web/FinalProject/ && sudo git checkout dev && sudo git pull'"
		}
                sleep 2
            }
        } 
        stage("run-docker-code-on-dev"){
            steps {
                sshagent (credentials: ['admin']) {
                    sh "ssh -o StrictHostKeyChecking=no e91user@3.91.19.106 'cd /home/e91user/web/FinalProject/ && sudo docker container stop \$(sudo docker container ls -aq) && sudo docker container rm \$(sudo docker container ls -aq) && sudo docker build -t dev-server . && sudo docker run -dit --name dev -p 80:80 dev-server'"
                }
                sleep 2
            }
        }
        stage("merge-dev-to-stage"){
            steps('Merge approval') {               
                sshagent (credentials: ['admin']) {

                    sh "ssh -o StrictHostKeyChecking=no e91user@3.91.19.106 'cd /home/e91user/web/FinalProject/ && sudo git pull origin dev && sudo git checkout . && sudo git checkout stage && sudo git merge remotes/origin/dev'" 
                }
                sleep 2
            }
    	}
       stage("pull-updates-to-stage"){
            steps {
                sshagent (credentials: ['admin']) {
                    sh "ssh -o StrictHostKeyChecking=no e91user@34.205.53.98 'cd /home/e91user/web/FinalProject/ && sudo git checkout stage && sudo git pull'"
		}
                sleep 2
            }
        } 
        stage("run-docker-code-on-stage"){
            steps {
                sshagent (credentials: ['admin']) {
                    sh "ssh -o StrictHostKeyChecking=no e91user@34.205.53.98 'cd /home/e91user/web/FinalProject/ && sudo docker container stop \$(sudo docker container ls -aq) && sudo docker container rm \$(sudo docker container ls -aq) && sudo docker build -t stage-server . && sudo docker run -dit --name stage -p 80:80 stage-server'"
                }
                sleep 2
            }
        }
        stage("merge-stage-to-master"){
            steps('Merge approval') {               
                sshagent (credentials: ['admin']) {

                    sh "ssh -o StrictHostKeyChecking=no e91user@34.205.53.98 'cd /home/e91user/web/FinalProject/ && sudo git pull origin stage && sudo git checkout . && sudo git checkout master && sudo git merge remotes/origin/stage'" 
                }
                sleep 2
            }
    	}
       stage("pull-updates-to-prod"){
            steps {
                sshagent (credentials: ['admin']) {
                    sh "ssh -o StrictHostKeyChecking=no e91user@35.245.200.195 'cd /home/e91user/web/FinalProject/ && sudo git checkout master && sudo git pull'"
		}
                sleep 2
            }
        } 
        stage("run-docker-code-on-prod"){
            steps {
                sshagent (credentials: ['admin']) {
                    sh "ssh -o StrictHostKeyChecking=no e91user@35.245.200.195 'cd /home/e91user/web/FinalProject/ && sudo docker container stop \$(sudo docker container ls -aq) && sudo docker container rm \$(sudo docker container ls -aq) && sudo docker build -t prod-server . && sudo docker run -dit --name prod -p 80:80 prod-server'"
                }
                sleep 2
            }
        }
    } 		 
}