#!groovy
def isPrToDevelop() {
    return (env.BRANCH_NAME =~ '^PR-\\d+' &&  env.CHANGE_TARGET == 'develop')
}
def BUILD_SUCCESS=true;
if (isPrToDevelop()) {
    print "isPrToDevelop"
}
else {
    print "No PrToDevelop"
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
        try {
        stage ('sonar Stage/static analysis') {
            steps {
                withSonarQubeEnv('SonarQube'){
                    sh 'pwd'
                    sh 'ls -ltr'
                    sh 'mvn sonar:sonar -DskipTests'
                }
            }
        }
        } catch (Exception exp) {
            BUILD_SUCCESS=false;
        }
        finally {
            print "=====Finally====="
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
