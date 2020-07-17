

node {
   // https://github.com/jfrog/project-examples/blob/master/jenkins-examples/pipeline-examples/scripted-examples/aql-example/Jenkinsfile
   /* ********************
    *
    * LOAD ENV VARIABLES
    *
    * ******************* */
   
   stage name: 'Load Env Variables'
   echo 'My build stage'
   git url: 'https://github.com/DonatoVerardi/artifactory-jenkins.git'
   load "env.groovy"
   
   /* ********************
    *
    * CONNECT TO ARTIFACTORY
    *
    * ******************* */
   stage name: 'Connect to Artifactory'
   // Get Artifactory server instance, defined in the Artifactory Plugin administration page.
    def server = Artifactory.server "${env.SERVER_ID}"
    def buildInfo = Artifactory.newBuildInfo()
    // Set custom build name and number.
    buildInfo.setName 'holyFrog'
    buildInfo.setNumber '42'
   
   /* ********************
    *
    * DOWNLOAD FILES
    *
    * ******************* */
    stage name: 'Download files from Artifactory'
    // The download file contains pattern for downloading artifacts to the root directory by setting recursive=false
   // def downloadSpec = readFile 'download.json'
   // Create the download spec.
    def downloadSpec = """{
         "files": [
                 {
                     "pattern": "${env.PATTERN}${env.FILENAME}.zip",
                     "target": "${env.TARGET}"
                  }
             ]
         }"""
    server.download spec: downloadSpec, buildInfo: buildInfo

    // Publish build info.
    server.publishBuildInfo buildInfo
   
    /* ********************
    *
    * SSH FILES
    *
    * ******************* */
   def remote = [:]
   remote.name = "ssh_container"
   remote.host = "ssh_container"
   remote.allowAnyHosts = true
   
    withCredentials([usernamePassword(credentialsId: 'deploy_key', passwordVariable: 'password', usernameVariable: 'userName')]) {
        remote.user = userName
        remote.password = password
       
       stage("SSH Steps Rocks!") {
            writeFile file: 'test.sh', text: 'ls'
            //sshCommand remote: remote, command: 'for i in {1..5}; do echo -n \"Loop \$i \"; date ; sleep 1; done'
            //sshScript remote: remote, script: 'test.sh'
            sshPut remote: remote, from: 'test.sh', into: '/var/tmp'
            //sshGet remote: remote, from: 'test.sh', into: 'test_new.sh', override: true
            //sshRemove remote: remote, path: 'test.sh'
        }
    }
}
