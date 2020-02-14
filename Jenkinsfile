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

   stage('Build image') {
      // Run the maven build

      echo "Build Image Maven"

      sh "docker build -t mymaven -f maven-dockerfile ."
   }

   stage('Build app') {

      echo "Build Docker Image App"
      sh "docker run -i -v $HOME/.m2:/root/.m2 -w /app/training-webapp/ --name test-maven  mymaven:v1.0 mvn clean install"
   }

   stage('Build image tomcat') {

      echo "Tomcat Container Configuration"
      sh "docker build -t mytomcat:v1.0 -f tomcat-dockerfile ."   
      
   }

   stage('Run Java Web application') {

      echo "Run Java Web application"
      sh "docker run -d -p 8888:8080 --name maven-webapp mytomcat:v1.0"
   }

}
