node{
  stage("Scm checkout"){
    git 'https://github.com/NguyenTienHCL/helloworld.git'
  }
  
  stage("Maven build"){
    sh 'mvn clean package'
  }
  
  stage("Build docker image"){
    sh 'docker build -t tiennguyenhcl/tomcat:v1 .'
  }
  
  stage("Push docker image"){
    withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerHubPwd')]) {
      sh "docker login -u anhdo98 -p ${dockerHubPwd}"
    }
    sh 'docker push tiennguyenhcl/tomcat:v1'
  }
  
//   stage("Remove"){
//     def dockerRm = 'docker container rm -f my-app-2'
//     sshagent(['dev-server']) {
//       sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.7.92 ${dockerRm}"
//     }
//   }
  
  stage("Deploy docker image to Tomcat server"){
    def dockerRun = 'docker run -p 8080:8080 -d --name web-test tiennguyenhcl/tomcat:v1'
    sshagent(['dev-server']) {
      sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.7.92 ${dockerRun}"
    }
  }
}
