 pipeline {
     agent {
         docker {
             image 'maven:3.6-jdk-8'
         }
     }
    stages {
    stage{
        steps('Clone project') {
            git 'https://github.com/mdeepevil/boxfuse-sample-java-war-hello.git'
        }
    }
    stage {
        steps('Build project'){
            sh 'mvn package -f ./pom.xml'
        }
    }
    stage('Build docker image and tag') {
        steps{
             sh 'docker build -t boxfuseapp:latest .' 
             sh 'docker tag boxfuseapp deepevil/boxfuseapp:latest'
        }
    }
    stage('Publish image to Docker Hub') {      
        steps {
              withDockerRegistry([ credentialsId: "dockerHub", url: "" ]) {
              sh  'docker push deepevil/boxfuseapp:latest'
        }
    }
    }
    stage('Run project on Jenkins node'){
        steps{
            sh "docker run -d -p 8005:8080 deepevil/boxfuseapp"
        }
    }
    stage('Run project on remote host'){
        steps{
            sh "docker -H ssh://jenkins@192.168.10.10 run -d -p 8005:8080 deepevil/boxfuseapp"
        }
    }
 }
 }