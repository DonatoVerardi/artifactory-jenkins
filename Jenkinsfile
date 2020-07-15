node {
   stage name: 'Load Env Variables'
   echo 'My build stage'
   git url: 'https://github.com/DonatoVerardi/artifactory-jenkins.git'
   load "env.groovy"
   echo "${env.DB_URL}"
   echo "${env.DB_URL2}"
   
   // https://github.com/jfrog/project-examples/blob/master/jenkins-examples/pipeline-examples/scripted-examples/aql-example/Jenkinsfile
   
   stage name: 'Connect to Artifactory'
   // Get Artifactory server instance, defined in the Artifactory Plugin administration page.
    def server = Artifactory.server "${env.SERVER_ID}"

    def buildInfo = Artifactory.newBuildInfo()
    // Set custom build name and number.
    buildInfo.setName 'holyFrog'
    buildInfo.setNumber '42'
   
    stage name: 'Download files from Artifactory'
    // The download file contains pattern for downloading artifacts to the root directory by setting recursive=false
    def downloadSpec = readFile 'aql-download.json'
    server.download spec: downloadSpec, buildInfo: buildInfo

    // Publish build info.
    //server.publishBuildInfo buildInfo
}
