node {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/walidsaad/training-app-gk.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'maven'
      jdkhome = tool 'java'
   }

   stage('Build') {
      // Run the maven build

      echo "Build Docker Image App"

      sh docker build -t="mymaven:v1.0"
      // withEnv(["MVN_HOME=$mvnHome", "JAVA_HOME=$jdkhome"]) {

      //    sh "'${mvnHome}/bin/mvn' -B archetype:generate -DarchetypeArtifactId=maven-archetype-webapp -DgroupId=com.mycompany.app -DartifactId=training-webapp"

      // }

   }
}
