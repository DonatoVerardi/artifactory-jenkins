node {
   stage name: 'Test'
   echo 'My build stage'
   git url: 'https://github.com/DonatoVerardi/artifactory-jenkins.git'
   load "env.groovy"
   echo "${env.DB_URL}"
   echo "${env.DB_URL2}"
}
