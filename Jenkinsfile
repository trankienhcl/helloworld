node{
  stage("Scm checkout"){
    git 'https://github.com/trankienhcl/helloworld.git'
  }
  
  stage("Maven build"){
    sh 'mvn clean package'
  }
  stage("Remove"){
     def dockerRm = 'docker container rm -f web-test'
     def dockerRm1 = 'docker image rm -f kayeofhallownest/imgtest1:latest'   
     sshagent(['ssh']) {
       sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.15.83 ${dockerRm}"
       sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.15.83 ${dockerRm1}"
     }
  }
  stage("Build docker image"){
    sh 'docker build -t kayeofhallownest/imgtest1:latest .'
  }
  
  /*stage("Push docker image"){
    sh "docker login -u kayeofhallownest -p pankaye1999"
    sh 'docker push kayeofhallownest/imgtest1:latest'
  } */
  stage("Deploy docker image to Tomcat server"){
    def dockerRun = 'docker run -p 8080:8080 -d --name web-test kayeofhallownest/imgtest1:latest'
    sshagent(['ssh']) {
      sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.15.83 ${dockerRun}"
    }
  }
}
