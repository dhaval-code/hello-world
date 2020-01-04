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

try {
stage ('Compile Stage') {
    node {
        withMaven(maven : 'MAVEN_HOME') {
            sh 'mvn clean compile'
        }
    }
}
} catch (Exception exp) {
    BUILD_SUCCESS=false;
} finally {
    print "====Finally===="
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
if (BUILD_SUCCESS) {
    print "Deploy Only if Build_SUCCESS"
}
stage ('Deployment Stage') {
    node {
        withMaven(maven : 'MAVEN_HOME') {
            sh 'mvn install'
        }
    }
}        
    

