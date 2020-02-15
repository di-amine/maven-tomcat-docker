node {
   //def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/di-amine/maven-tomcat-docker.git'
      // // Get the Maven tool.
      // // ** NOTE: This 'M3' Maven tool must be configured
      // // **       in the global configuration.           
      // mvnHome = tool 'maven'
      // jdkhome = tool 'java'
   }

   stage ('Build Vagrant Box') {
      // if base box isn't here, install it
      echo 'Building vagrant VM...'
      sh "vagrant up"
   }

   stage('Build image') {
      // Run the maven build

      echo "Build Image Maven"

      sh "docker build -t mymaven -f maven-dockerfile ."
   }

   stage('Build app') {

      echo "Build Docker Image App"
      sh "docker run -i -v $HOME/.m2:/root/.m2 -w /app/training-webapp/ --name test-maven  mymaven:v1.0 mvn clean install"
   }

   stage('Copy Jar') {
      // Run the maven build

      echo "Copy Jar"

      sh "cp /var/lib/jenkins/.m2/repository/com/mycompany/app/training-webapp/1.0-SNAPSHOT/training-webapp-1.0-SNAPSHOT.war ."
   }

   stage('Build image tomcat') {

      echo "Tomcat Container Configuration"
      sh "docker build -t mytomcat:v1.0 -f tomcat-docker ."   
      
   }

   stage('Run Java Web application') {

      echo "Run Java Web application"
      sh "docker run -d -p 8888:8080 --name maven-webapp mytomcat:v1.0"
   }

}
