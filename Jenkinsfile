pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                
                echo 'Hello World'
        
                
              
                sh "ansible-playbook -vvvv ${WORKSPACE}/run_setup.yml -e env_to_run=Docker"
            }
            
        }
    }
}
