pipeline {
    agent { label 'master' }
    stages {
        stage('Build Dev') {
            steps {
                echo "Hello Dev!"
            }
        }
        stage('Build Stage') {
            steps {
                echo "Hello Stage!"
            }
        }
        stage('Build Prod') {
            steps {
                echo "Hello Prod!"
            }
        }
    }
}