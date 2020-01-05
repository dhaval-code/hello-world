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

if (BUILD_SUCCESS) {
    print "Deploy Only if Build_SUCCESS"

stage ('Deployment Stage') {
    node {
        withMaven(maven : 'MAVEN_HOME') {
            sh 'mvn clean install'
        }
    }
}
} else {
    print "==== Error in Pipeline ===="
}

    

