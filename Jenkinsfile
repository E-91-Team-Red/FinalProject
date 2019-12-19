pipeline {
    agent any
    stages {
        stage("pull-updates-to-dev"){
            steps {
                sshagent (credentials: ['admin']) {
                    sh "ssh -o StrictHostKeyChecking=no e91user@3.230.126.252 'cd /home/e91user/web/FinalProject/ && sudo git checkout dev1 && sudo git pull'"
		}
                sleep 2
            }
        } 
        stage("run-docker-code-on-dev"){
            steps {
                sshagent (credentials: ['admin']) {
                    sh "ssh -o StrictHostKeyChecking=no e91user@3.230.126.252 'cd /home/e91user/web/FinalProject/ && sudo docker container stop \$(sudo docker container ls -aq) && sudo docker container rm \$(sudo docker container ls -aq) && sudo docker build -t dev-server . && sudo docker run -dit --name dev -p 80:80 dev-server'"
                }
                sleep 2
            }
        }
        stage("merge-dev-to-stage"){
            steps('Merge approval') {               
                sshagent (credentials: ['admin']) {
                    sh "ssh -o StrictHostKeyChecking=no e91user@34.205.155.170 'cd /home/e91user/web/FinalProject/ && sudo git pull origin dev1 && sudo git checkout . && sudo git checkout stage1 && sudo git merge remotes/origin/dev1 && sudo git push origin stage1'" 
                }
                sleep 2
            }
    	}
       stage("pull-updates-to-stage"){
            steps {
                sshagent (credentials: ['admin']) {
                   sh "ssh -o StrictHostKeyChecking=no e91user@34.205.155.170 'cd /home/e91user/web/FinalProject/ && sudo git checkout stage1 && sudo git pull origin'"
		}
                sleep 2
            }
        } 
        stage("run-docker-code-on-stage"){
            steps {
                sshagent (credentials: ['admin']) {
                   sh "ssh -o StrictHostKeyChecking=no e91user@34.205.155.170 'cd /home/e91user/web/FinalProject/ && sudo docker container stop \$(sudo docker container ls -aq) && sudo docker container rm \$(sudo docker container ls -aq) && sudo docker build -t stage-server . && sudo docker run -dit --name stage -p 80:80 stage-server'"
                }
                sleep 2
            }
        }
        stage("merge-stage-to-master"){
            steps('Merge approval') {               
                sshagent (credentials: ['admin']) {

                    sh "ssh -o StrictHostKeyChecking=no e91user@35.245.200.195 'cd /home/e91user/web/FinalProject/ && sudo git pull origin stage1 && sudo git checkout . && sudo git checkout master && sudo git merge remotes/origin/stage1 && sudo git push origin master'" 
                }
                sleep 2
            }
    	}
        stage("pull-updates-to-prod"){
            steps {
                sshagent (credentials: ['admin']) {
                    sh "ssh -o StrictHostKeyChecking=no e91user@35.245.200.195 'cd /home/e91user/web/FinalProject/ && sudo git checkout master && sudo git pull origin'"
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
 