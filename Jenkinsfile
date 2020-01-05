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
stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
    node {
        checkout scm
    }
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
/*stage('Sanity check') {
    node {
        input "Does the staging environment look ok?"
    }
}
stage('Deploy - Production') {
    node {
        //prod deploy steps
        print '===== Deploy to Prod ====='
    }
} */

def remote = [:]
remote.name = "centos-ansible"
remote.host = "192.168.1.239"
remote.allowAnyHosts = true
stage('ssh into server') {
    node {
        withCredentials([usernamePassword(credentialsId: 'ansible-host', passwordVariable: 'password', usernameVariable: 'userName')]) {
            remote.user = userName
            remote.password = password
            //sh 'pwd'
            //sh 'ls -ltr'
            stage("SSH Steps Rocks!") {
                //writeFile file: 'test.sh', text: 'ls'
                sshCommand remote: remote, command: 'pwd'
                sshCommand remote: remote, command: 'cd /opt/tomcat'
                //sshScript remote: remote, script: 'test.sh'
                //sshPut remote: remote, from: 'test.sh', into: '.'
                //sshGet remote: remote, from: 'test.sh', into: 'test_new.sh', override: true
                //sshRemove remote: remote, path: 'test.sh'
            }
        }
    }
}
    

