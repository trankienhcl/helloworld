node{
  stage("Scm checkout"){
    git 'https://github.com/trankienhcl/helloworld.git'
  }
  
  stage("Maven build"){
    sh 'mvn clean package'
  }
  
  stage("Build docker image"){
    sh 'docker build -t kayeofhallownest/text1:v1 .'
  }
  
  stage("Push docker image"){
    withCredentials([string(credentialsId: '0101', variable: 'docker-test')]) {
      sh "docker login -u kayeofhallownest -p ${docker-test}"
    }
    sh 'docker push kayeofhallownest/text1:v1'
  }
  
//   stage("Remove"){
//     def dockerRm = 'docker container rm -f my-app-2'
//     sshagent(['dev-server']) {
//       sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.7.92 ${dockerRm}"
//     }
//   }
  
  stage("Deploy docker image to Tomcat server"){
    def dockerRun = 'docker run -p 8080:8080 -d --name web-test kayeofhallownest/text1:v1'
    sshagent(['dev-server']) {
      sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.15.83 ${dockerRun}"
    }
  }
}
