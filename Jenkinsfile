pipeline {
    agent any

    tools {
        maven "maven 3.9.5"
    }

    environment {
        DOCKER_USERNAME = credentials('DOCKER_USERNAME')
        DOCKER_PASSWORD = credentials('DOCKER_PASSWORD')
        DOCKER_IMAGE = "esra/java:v-$BUILD_TAG"
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn  -B -DskipTests clean package'
            

            post {
                success {
                archiveArtifacts artifacts: 'java-app/target/*.jar', fingerprint: true
                }
            }
        }        

        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                junit 'java-app/target/surefire-reports/*.xml', fingerprint: true

                }
            }
        }

        stage('Docker-image') {
            steps {
                sh 'docker image build -t $DOCKER_IMAGE .'
            }
        }

        stage('Login-Dockerhub') {
            steps {
                sh '''
                    docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
                    docker image push $DOCKER_IMAGE
                '''

            }
        }

        stage('create-container') {
            steps {
                sh 'docker container run --name frontend -p 80:80 $$DOCKER_IMAGE '
            }
        }        
    }
}
