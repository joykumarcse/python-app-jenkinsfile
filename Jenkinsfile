node{
   stage('SCM Checkout'){
       git 'https://github.com/joyktech/python-app-jenkinsfile'
   }
   stage('build Docker Image'){
     sh 'docker build -t joyktech/my-testpython:2.0.0 .'
   }
   stage('Push Docker Image'){
     withCredentials([string(credentialsId: 'dockerpassword', variable: 'dockerhubpassword')]) {
        sh "docker login -u joyktech -p ${dockerhubpassword}"
     }
     sh 'docker push joyktech/my-testpython:2.0.0'
   }
   stage('Run Container on Staging'){ 
     def dockerRun = 'docker run  -p 6379:6379 -d --name redis redis'
     def dockerRun1 = 'docker run -p 4040:80 -d --link redis --name my-python-app joyktech/my-testpython:2.0.0'
     def dockerRun2 = 'docker rm -f my-pyhton-app'
     sshagent(['dockerserver']) {
       sh "ssh -o StrictHostKeyChecking=no root@172.16.20.213 ${dockerRun}"
       sh "ssh -o StrictHostKeyChecking=no root@172.16.20.213 ${dockerRun2}"
       sh "ssh -o StrictHostKeyChecking=no root@172.16.20.213 ${dockerRun1}"
     }
   }
}
