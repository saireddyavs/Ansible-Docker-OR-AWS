pipeline {
  agent any




  stages {

    stage('CheckoutApplication') {
      steps {
        sh 'mkdir -p application'
        dir("application")
        {
          git(url: 'https://github.com/saireddyavs/Gym-appication.git')
        }
      }
    }

    stage('CheckoutDEV') {
      steps {
        sh 'mkdir -p dev'
        dir("dev")
        {
          git(url: 'https://github.com/saireddyavs/Ansible-Docker-OR-AWS.git/')
        }
      }
    }

    stage('copy sonar properties to workspace'){
      steps{
        sh 'cp ${WORKSPACE}/dev/sonar-project.properties ${WORKSPACE}/'
      }
    }
    stage('SonarQube') {

      steps {
        script {
          // requires SonarQube Scanner 2.8+
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




        sh "ansible-playbook ${WORKSPACE}/dev/run_setup.yml -e ansible_become_pass=yoursystempass -e env_to_run=Docker"
      }

    }


  }
}
