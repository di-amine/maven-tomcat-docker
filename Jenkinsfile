node {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git credentialsId: '6712898c-10b9-41df-b6ac-7193b452723c', url: 'https://github.com/di-amine/maven-tomcat-docker.git'
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
      sh "sudo chmod 777 /var/lib/jenkins/workspace/pipeline"
   }

   stage ('Provisionnig VM Vagrant Box to install Docker') {
      // if base box isn't here, install it
      echo 'Install Docker to VM MAchine.'
      sh "sudo ansible-playbook -i hosts.ini dockerInstall.yml"
   }

   node ('app') {

      stage('Preparation') { 
         
      git 'https://github.com/di-amine/maven-tomcat-docker.git'

      }

      stage('Build image') {
         // Run the maven build

         echo "Build Image Maven"

         sh "docker build -t mymaven -f maven-dockerfile ."
      }

      stage('Build app') {

         echo "Build Docker Image App"
         sh "docker run -i -v /home/vagrant/jenkins/.m2:/root/.m2 -w /jenkins/app/training-webapp/ --name test-maven  maven:3.3-jdk-8 mvn clean install"
      }

      stage('Copy Jar') {
         // Run the maven build

         echo "Copy Jar"

         sh "cp /var/lib/$USER/.m2/repository/com/mycompany/app/training-webapp/1.0-SNAPSHOT/training-webapp-1.0-SNAPSHOT.war ."
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


}
