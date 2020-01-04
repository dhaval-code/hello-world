pipeline {
    agent any

    stages {
        stage ('Compile Stage') {

            node {
                withMaven(maven : 'MAVEN_HOME') {
                    sh 'mvn clean compile'
                }
            }
        }

        stage ('Testing Stage') {

            node {
                withMaven(maven : 'MAVEN_HOME') {
                    sh 'mvn test'
                }
            }
        }
        stage ('sonar Stage/static analysis') {
            node {
                withSonarQubeEnv('SonarQube'){
                    sh 'pwd'
                    sh 'ls -ltr'
                    sh 'mvn sonar:sonar -DskipTests'
                }
            }
        }

        stage ('Deployment Stage') {
            node {
                withMaven(maven : 'MAVEN_HOME') {
                    sh 'mvn install'
                }
            }
        }
        
    }
}
