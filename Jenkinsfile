pipeline {
    agent none
    stages {
        stage('Maven Install') {
            agent {
                docker {
                    image 'maven:3.9-eclipse-temurin-25'
                    reuseNode true
                }
            }
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Docker Build') {
            agent any
            steps {
                sh 'docker build -t juanexzz/spring-petclinic:gestion-udem-jenkins .'
            }
        }
        stage('Docker Push') {
            agent any
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerHub', usernameVariable: 'dockerHubUser', passwordVariable: 'dockerHubPassword')]) {
                    sh 'echo "$dockerHubPassword" | docker login -u "$dockerHubUser" --password-stdin'
                    sh 'docker push juanexzz/spring-petclinic:gestion-udem-jenkins'
                }
            }
        }
    }
}
