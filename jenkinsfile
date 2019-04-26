node {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/ernesrufin/training-app'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'Maven3'
   }
   
    stage('Test') {
      if (isUnix()) {
         sh "'${mvnHome}/binmvn' test"
   } else {
      bat (/"${mvnHome}\bin\mvn" test/)
   }
   }
   
   stage('Build') {
      // Run the maven build
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
      } else {
         bat(/"${mvnHome}\bin\mvn" -B -DskipTests -Dmaven.test.failure.ignore package/)
      }
   }
  
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archiveArtifacts 'target/*.jar'
   }
   stage('publish Artefact') {
   if (isUnix()) {
      sh "'${mvnHome}/bin/mvn' -DdeployOnly deploy"
   }  else {
      //bat(/"${mvnHome}\bin\mvn" -DdeployOnly deploy -s "C:\Program Files (x86)\apache-maven-3.6.0\conf\settings.xml")
   }
}

stage('Execute jar') {
   if (isUnix()) {
      sh "java -jar target/*.jar"
   } else {
      bat(/java -jar target\/training-app-1.0-SNAPSHOT-jar-with-dependencies.jar/)
   }
}
}
