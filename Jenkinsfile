node{
  stage("Scm checkout"){
    git 'https://github.com/trankienhcl/helloworld.git'
  }
  
  stage("Maven build"){
    sh 'mvn clean package'
  }
  
  stage("Build docker image"){
    sh 'docker build -t kayeofhallownest/text1:v2 .'
  }
  
  stage("Push docker image"){
    sh "docker login -u kayeofhallownest -p pankaye1999"
    sh 'docker push kayeofhallownest/text1:v2'
  }
  
  stage("Remove"){
     def dockerRm = 'docker container rm -f text1'
     sshagent(['ssh']) {
       sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.15.83 ${dockerRm}"
     }
   }
  
  stage("Deploy docker image to Tomcat server"){
    def dockerRun = 'docker run -p 8080:8080 -d --name web-test kayeofhallownest/text1:v2'
    sshagent(['ssh']) {
      sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.15.83 ${dockerRun}"
    }
  }
}
