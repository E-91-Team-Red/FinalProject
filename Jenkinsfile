  
pipeline {
    agent any

    stages {
        stage("pull-updates-to-dev"){
            steps {
                sshagent (credentials: ['admin']) {
                    sh "ssh -o StrictHostKeyChecking=no e91user@3.216.80.220 'cd /home/e91user/web/FinalProject/ && git checkout . && git checkout dev && git pull'"
        }
                sleep 2
            }
        }    
        stage("run-docker-code-on-dev"){
            steps {
                sshagent (credentials: ['admin']) {
                    sh "docker container stop $(docker container ls -aq)"
                    sh "docker container rm $(docker container ls -aq)"
                    sh "docker build -t dev-server ."
                    sh "docker run -dit --name dev -p 80:80 dev-server"
                }
                sleep 2
            }
        }
        stage("check-output-on-dev"){
            steps {
                sshagent (credentials: ['admin']) {
           echo "check output on dev"
                }
                sleep 2
            }
        }
        stage("merger-dev-to-stage"){
            steps('Merge approval') {
            input ("Merge dev to master? ")
               
                sshagent (credentials: ['admin']) {

                    echo "merge to stage"
                }
                sleep 2
            }
        }
    }        
}