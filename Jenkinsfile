pipeline {
    agent any

    stages {
        stage('Git checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/spring-petclinic/spring-framework-petclinic.git'
            }
        }
        stage('Maven test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Sonar-scanner') {
            steps {
                withSonarQubeEnv(installationName:'Sonarqube', credentialsId: 'sonar') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Build') {
            steps {
                sh 'mvn package'
            }
        }
        stage('Nexus-uploader') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'spring-framework-petclinic', classifier: '', file: 'target/petclinic.war', type: 'war']], credentialsId: 'nexus', groupId: 'org.springframework.samples', nexusUrl: '3.84.18.238:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'Petclinic', version: '6.2.8'
            }
        }
        stage('Tomcat-deplpy') {
            steps {
                sh 'scp target/petclinic.war ubuntu@172.31.87.237:/home/ubuntu/apache-tomcat-10.1.44/webapps'
            }
        }
    }
}
