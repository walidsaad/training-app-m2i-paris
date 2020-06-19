node {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/walidsaad/training-app-demo.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'maven3'
   }
 
        
   stage('Build') {
      // Run the maven build
      if (isUnix()) {
          
         sh "export JAVA_HOME=/usr/lib/jvm/java-11-openjdk && '${mvnHome}/bin/mvn' -B -DskipTests -Dmaven.test.failure.ignore clean package"
      } else {
         bat(/"${mvnHome}\bin\mvn" -B -DskipTests -Dmaven.test.failure.ignore clean package/)
      }
   }
   
   stage('Test') {
      if (isUnix()) {
          
         sh "export JAVA_HOME=/usr/lib/jvm/java-11-openjdk && '${mvnHome}/bin/mvn' test"
      } else {
         bat(/"${mvnHome}\bin\mvn" test/)
      }
   }
   
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archiveArtifacts  'target/*.jar'
   }
   
    stage('Publish Artefact') {
        
      if (isUnix()) {
         sh "export JAVA_HOME=/usr/lib/jvm/java-11-openjdk && '${mvnHome}/bin/mvn' -Dmaven.test.skip=true deploy -s /home/stagiaire/tools/apache-maven-3.6.3/conf/settings.xml"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.skip=true deploy -s "C:\Program Files (x86)\apache-maven-3.5.3\conf\settings.xml"/)
      }
   }
   
   stage('Execute Jar') {
      if (isUnix()) {
         sh "java -jar  target/training-app-1.0-SNAPSHOT-jar-with-dependencies.jar"
      } else {
         bat(/java -jar target\/training-app-1.0-SNAPSHOT-jar-with-dependencies.jar/)
      }
   }
}
