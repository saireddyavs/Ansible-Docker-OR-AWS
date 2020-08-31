pipeline {
  agent any


  environment {
    WHERE_TO_RUN = 'Docker'
    SYSTEM_PASSWORD = 'system_password'

  }






  stages {

    stage("Clean WorkSpace"){
          steps{
              cleanWs()
          }
      }

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

    stage('Running Ansible Playbook') {
      steps {

        echo 'Hello World'




        sh "ansible-playbook ${WORKSPACE}/dev/run_setup.yml -e ansible_become_pass=${SYSTEM_PASSWORD} -e env_to_run=${WHERE_TO_RUN}"
      }

    }

    stage('Running Selenium Tests in AWS') {

      when {
        // Only say hello if a "WHRE_TO_RUN" is AWS
        environment name: 'WHERE_TO_RUN', value: 'AWS'
      }


      steps {

        sh 'mkdir -p test'
        dir('test'){

        git 'https://github.com/saireddyavs/selenium_gym_app.git'
        script {
          env.deployed_ip = sh(script: "http://" + "sed -n '/webservers/{n;p}' ${WORKSPACE}/dev/hosts", returnStdout: true).trim() + "/"

          echo "MYVAR: ${env.deployed_ip}"
        }
        dir("test2"){
          sh "mvn clean compile test -DIPDEPLOYED=${deployed_ip}"

          step([$class: 'Publisher', reportFilenamePattern: ' **/test-ouput/testng-results.xml'])
        }

        }
      }

    }

    stage('Running Selenium Tests in Docker') {

      when {
        // Only say hello if a "WHRE_TO_RUN" is Docker
        environment name: 'WHERE_TO_RUN', value: 'Docker'
      }


      steps {

        sh 'mkdir -p test'

        dir('test'){

        git 'https://github.com/saireddyavs/selenium_gym_app.git'
        script {
          env.deployed_ip = "https://localhost/"

          echo "MYVAR: ${env.deployed_ip}"
        }
        dir("test2"){
          sh "mvn clean compile test -DIPDEPLOYED=${deployed_ip}"

          step([$class: 'Publisher', reportFilenamePattern: ' **/test-ouput/testng-results.xml'])
        }

      }
      }

    }

    stage('Artifactory configuration') {
      when {
        // Only say hello if a "WHRE_TO_RUN" is Docker
        environment name: 'WHERE_TO_RUN', value: 'Docker'
      }
      steps {
        rtServer(
          id: "Artifactory-1",
          username: 'admin',
          password: 'artifactory_password',


          url: 'http://localhost:8081/artifactory',

        )
      }
    }


    stage('Push image to Artifactory') {
      when {
        // Only say hello if a "WHRE_TO_RUN" is Docker
        environment name: 'WHERE_TO_RUN', value: 'Docker'
      }
      steps {
        rtDockerPush(

          serverId: "Artifactory-1",

          image: 'localhost:8081/docker-local/proj_mysql',
          // Host:
          // On OSX: "tcp://127.0.0.1:1234"
          // On Linux can be omitted or null

          targetRepo: 'docker-local',
          // Attach custom properties to the published artifacts:
          properties: 'project-name=docker1-${JOB_NAME}-${BUILD_NUMBER};status=stable'
        )
        rtDockerPush(

          serverId: "Artifactory-1",

          image: 'localhost:8081/docker-local/proj_backend',
          // Host:
          // On OSX: "tcp://127.0.0.1:1234"
          // On Linux can be omitted or null

          targetRepo: 'docker-local',
          // Attach custom properties to the published artifacts:
          properties: 'project-name=docker1-${JOB_NAME}-${BUILD_NUMBER};status=stable'
        )
        rtDockerPush(

          serverId: "Artifactory-1",

          image: 'localhost:8081/docker-local/proj_proxy',
          // Host:
          // On OSX: "tcp://127.0.0.1:1234"
          // On Linux can be omitted or null

          targetRepo: 'docker-local',
          // Attach custom properties to the published artifacts:
          properties: 'project-name=docker1-${JOB_NAME}-${BUILD_NUMBER};status=stable'
        )
      }
    }

    stage('Publish build info') {
      steps {
        rtPublishBuildInfo(
          serverId: "Artifactory-1"
        )
      }
    }



  }
}
