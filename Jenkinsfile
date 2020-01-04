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
    

