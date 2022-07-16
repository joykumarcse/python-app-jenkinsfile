node{
   stage('SCM Checkout'){
       git 'https://github.com/jahirshawon/python-app-jenkinsfile'
   }
   stage('build Docker Image'){
     sh 'docker build -t jahirshawon/my-testpython:2.0.0 .'
   }
   stage('Push Docker Image'){
     withCredentials([string(credentialsId: 'dockerpassword', variable: 'dockerhubpassword')]) {
        sh "docker login -u jahirshawon -p ${dockerhubpassword}"
     }
     sh 'docker push jahirshawon/my-testpython:2.0.0'
   }
   stage('Run Container on Staging'){ 
     def dockerRun = 'docker run  -p 6379:6379 -d --name redis redis'
     def dockerRun1 = 'docker run -p 4040:80 -d --link redis --name my-python-app jahirshawon/my-testpython:2.0.0'
     def dockerRun2 = 'docker rm -f my-pyhton-app'
     sshagent(['dockerserver4']) {
       sh "ssh -o StrictHostKeyChecking=no root@192.168.43.244 ${dockerRun}"
       sh "ssh -o StrictHostKeyChecking=no root@192.168.43.244 ${dockerRun2}"
       sh "ssh -o StrictHostKeyChecking=no root@192.168.43.244 ${dockerRun1}"
     }
   }
}
