pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                
                echo 'Hello World'
                
                git("https://github.com/saireddyavs/Ansible-mysql-flask-EC2-nagios")
                
              
                sh "ansible-playbook -vvvv ${WORKSPACE}/run_setup.yml"
            }
            
        }
    }
}
