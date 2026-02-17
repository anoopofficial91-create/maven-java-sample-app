pipeline {
    agent { label 'worker' }

    tools {
        jdk 'java'
        maven 'maven'
    }

    stages {
        stage('git checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/anoopofficial91-create/maven-java-sample-app.git'
            }
        }

        stage('compile') {
            steps {
                sh 'mvn spring-javaformat:apply'
                sh 'mvn clean compile'
            }
        }

        stage('build') {
            steps {
                sh 'mvn package'
            }
        }

        stage('push docker image') {
            steps {
                script {
                    def dockerImage = 'anoop8cd/maven-java-sample-app'
                    def dockerTag = 'latest'

                   withCredentials([usernamePassword(credentialsId: 'docker123', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh "docker login -u $DOCKER_USER -p $DOCKER_PASS"
                   }

                    sh "docker build -t ${dockerImage}:${dockerTag} ."
                    sh "docker push ${dockerImage}:${dockerTag}"
                }
            }
        }

        stage('deploy application') {
            steps {
                script {
                    def dockerImage = 'anoop8cd/maven-java-sample-app'
                    def dockerTag = 'latest'

                    sh "docker pull ${dockerImage}:${dockerTag}"
                    sh "docker run -d -p 8001:8001 --name maven-app ${dockerImage}:${dockerTag}"
                }
            }
        }
    }
}
