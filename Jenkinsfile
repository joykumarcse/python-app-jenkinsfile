node{
   stage('SCM Checkout'){
       git 'https://github.com/joyktech/python-app-jenkinsfile.git'
   }
   stage('build Docker Image'){
     sh 'docker build -t joyktech/my-testpython:2.0.4 .'
   }
   stage('Push Docker Image'){
     withCredentials([string(credentialsId: 'dockerpassword', variable: 'dockerhubpassword3')]) {
        sh "docker login -u joyktech -p ${dockerhubpassword3}"
     }
     sh 'docker push joyktech/my-testpython:2.0.4'
   }
   stage('Run Container on Staging'){ 
     def dockerRun = 'docker run  -p 6378:6379 -d --name redis redis'
     def dockerRun1 = 'docker run -p 4042:80 -d --link redis --name my-python-app joyktech/my-testpython:2.0.4'
     def dockerRun2 = 'docker rm -f my-python-app'
     def dockerRun3 = 'docker rm -f redis'
     sshagent(['dockerserver3']) {
       sh "ssh -o StrictHostKeyChecking=no -p 2209 root@172.16.20.215 ${dockerRun3}"
       sh "ssh -o StrictHostKeyChecking=no -p 2209 root@172.16.20.217 ${dockerRun2}"
       sh "ssh -o StrictHostKeyChecking=no -p 2209 root@172.16.20.217 ${dockerRun}"
       sh "ssh -o StrictHostKeyChecking=no -p 2209 root@172.16.20.217 ${dockerRun1}"
         
     }
   }
}
