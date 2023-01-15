node{
   stage('SCM Checkout'){
       git 'https://github.com/joykumarhub/python-app-jenkinsfile.git'
   }
   stage('build Docker Image'){
     sh 'docker build -t joyktech/hello-world:1.0 .'
   }
   stage('Push Docker Image'){
     withCredentials([string(credentialsId: 'dockerpassword', variable: 'dockerhubjoy')]) {
        sh "docker login -u joyktech -p ${dockerhubjoy}"
     }
     sh 'docker push joyktech/hello-world:1.0'
   }
   stage('Run Container on Staging'){ 
     def dockerRun = 'docker run  -p 6378:6379 -d --name redis redis'
     def dockerRun1 = 'docker run -p 4042:80 -d --link redis --name my-python-app joyktech/hello-world:1.0'
     def dockerRun2 = 'docker rm -f my-python-app'
     def dockerRun3 = 'docker rm -f redis'
     sshagent(['dockerserver']) {
       sh "ssh -o StrictHostKeyChecking=no root@192.168.10.232 ${dockerRun3}"
       sh "ssh -o StrictHostKeyChecking=no root@192.168.10.232 ${dockerRun2}"
       sh "ssh -o StrictHostKeyChecking=no root@192.168.10.232 ${dockerRun}"
       sh "ssh -o StrictHostKeyChecking=no root@192.168.10.232 ${dockerRun1}"
         
     }
   }
}
