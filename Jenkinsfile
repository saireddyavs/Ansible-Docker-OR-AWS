pipeline {
    agent any
    
    


    stages {
         stage('Checkout code') {
            steps {
               git(url: 'https://github.com/saireddyavs/Ansible-Docker-OR-AWS.git/')
            }
        }
       stage('SonarQube') {
           
          steps {
              script {
        
          scannerHome = tool 'sonar-scanner'
        }
                    
                    withSonarQubeEnv('SonarQube') {
                      sh "${scannerHome}/bin/sonar-scanner"
                    }
                  }
                }
              
        stage('Hello') {
            steps {
                
                echo 'Hello World'
                
                
                
            
                sh "ansible-playbook ${WORKSPACE}/run_setup.yml -e ansible_become_pass=rgukt123 -e env_to_run=Docker"
            }
            
        }
        
        
    }
}
