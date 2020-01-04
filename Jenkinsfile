#!groovy
def isPrToDevelop() {
    return (env.BRANCH_NAME =~ '^PR-\\d+' &&  env.CHANGE_TARGET == 'develop')
}
def BUILD_SUCCESS=false;
if (isPrToDevelop()) {
    //sh 'echo \"Hello Pipeline !! \"'
}
else {
        sh 'echo \" Hello Pipeline!! \"'
}

pipeline {
    agent any

    stages {
        stage ('Compile Stage') {

            steps {
                withMaven(maven : 'MAVEN_HOME') {
                    sh 'mvn clean compile'
                }
            }
        }

        stage ('Testing Stage') {

            steps {
                withMaven(maven : 'MAVEN_HOME') {
                    sh 'mvn test'
                }
            }
        }
        stage ('sonar Stage/static analysis') {
            steps {
                withSonarQubeEnv('SonarQube'){
                    sh 'pwd'
                    sh 'ls -ltr'
                    sh 'mvn sonar:sonar -DskipTests'
                }
            }
        }

        stage ('Deployment Stage') {
            steps {
                withMaven(maven : 'MAVEN_HOME') {
                    sh 'mvn install'
                }
            }
        }
        
    }
}
