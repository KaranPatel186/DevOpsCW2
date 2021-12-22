node {
    def app

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Build image') {
  
       app = docker.build("karanpatel186/devopscw2")
    }

    stage('Test image') {

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        
        docker.withRegistry('https://registry.hub.docker.com', 'git') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
    
    stage('Deploy to K8s') {
        
     steps{
         
     sshagent(['k8s-jenkins'])
     script{
      try{
       sh 'ssh ubuntu@ip-172-31-18-146 kubectl create deployment DevOpsCW2 --image=karanpatel186/devopscw2'
      }catch(error)
        }
     }
    }
   }
    
