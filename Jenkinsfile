node{
   stage('SCM Checkout'){
       git 'https://github.com/joyktech/python-app-jenkinsfile'
   }
   stage('build Docker Image'){
     sh 'docker build -t joyktech/my-testpython:2.0.2 .'
   }
   stage('Push Docker Image'){
     withCredentials([string(credentialsId: 'dockerpassword', variable: 'dockerhubpassword')]) {
        sh "docker login -u joyktech -p ${dockerhubpassword}"
     }
     sh 'docker push joyktech/my-testpython:2.0.2'
   }
   stage('Run Container on Staging'){ 
     def dockerRun = 'docker run  -p 6378:6379 -d --name redis redis'
     def dockerRun1 = 'docker run -p 4041:80 -d --link redis --name my-python-app joyktech/my-testpython:2.0.2'
     def dockerRun2 = 'docker rm -f my-python-app'
     def dockerRun3 = 'docker rm -f redis'
     sshagent(['dockerserver']) {
       sh "ssh -o StrictHostKeyChecking=no -p 2209 root@172.16.20.217 ${dockerRun3}"
       sh "ssh -o StrictHostKeyChecking=no -p 2209 root@172.16.20.217 ${dockerRun2}"
       sh "ssh -o StrictHostKeyChecking=no -p 2209 root@172.16.20.217 ${dockerRun}"
       sh "ssh -o StrictHostKeyChecking=no -p 2209 root@172.16.20.217 ${dockerRun1}"
       sh 'ls'   
     }
   }
}
