node (label: 'stageenv'){     
  def app
  stage('Clone repository') 
     {         /* Make Sure you have a repository Created */          
       checkout scm    
     }      
  stage('Build image') 
     {                
       app = docker.build("saidocker01/nginx")     
     }      
  stage('Push image') {
       docker.withRegistry('https://registry.hub.docker.com', 'saidockerhubcreds') {            
       app.push("${env.BUILD_NUMBER}")            
       app.push("latest")        
              }    
  }
  stage('Test image') 
  {         
       app.inside {             
          sh 'echo "Tests passed"'         
                 }     
  }      
  stage('Deploy image on Staging Environment') 
  {
        sh ''' echo "Deploying Container Image" '''
        docker.withRegistry('https://registry.hub.docker.com', 'saidockerhubcreds')  {
        docker.image("saidocker01/nginx:${env.BUILD_NUMBER}").run('--name nginx -p 80:80 -d')
        sh '''
        sleep 5
        echo "Checking if WebServer is working Post deployment"
        curl http://localhost
        docker stop nginx
        docker rm nginx
        '''
        }  
  } 
}
node (label: 'prodenv'){
  stage('Deploy image on Production Environment') 
  {
        sh ''' echo "Deploying Container Image" '''
        docker.withRegistry('https://registry.hub.docker.com', 'saidockerhubcreds')  {
        docker.image("saidocker01/nginx:${env.BUILD_NUMBER}").run('--name nginx -p 80:80 -d')
        sh '''
        sleep 5
        echo "Checking if WebServer is working Post deployment"
        curl http://localhost
        docker stop nginx
        docker rm nginx
        '''
        }  
  }
}
