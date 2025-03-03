pipeline {
   //agent { label 'agente1' } //se coloca el agente que qconfiguraste en jenkins
   agent any

  stages {
    stage('Verificar Docker') {
      steps {
        sh 'docker info'
      }
    }
    // stage('dar permisos'){
    //   steps{
    //     sh 'chmod +x ./vendor/bin/phpunit'
    //   }
    // }

    // stage('Docker build'){
    //   steps{
    //     sh 'docker build -t jenkins-laravel .'
    //   }
    // }

    // stage('Run test'){
    //   steps{
    //     sh 'docker run --rm jenkins-laravel ./vendor/bin/phpunit tests'
    //   }
    // }

     stage('Sonarqube') {
       steps {
         script {
           docker.image('sonarsource/sonar-scanner-cli').inside('--network ci-network') {
             sh '''
             sonar-scanner\
              -Dsonar.host.url=http:sonarqube:9000\
              -Dsonar.projectKey=my-php-app \
              -Dsonar.sources=src \
              -Dsonar.login=squ_9beda2e4c9243ea4784f68dea8c3a850b79a868c
             '''
           }
         }
       }
     }

    // stage('Docker build') {
    //   steps {
    //     sh 'docker build -t jenkins-laravel .'
    //   }
    // }

    // stage('Run test') {
    //   steps {
    //     sh 'docker run --rm jenkins-laravel ./vendor/bin/phpunit tests'
    //   }
    // }

     stage('Deploy') {
       steps {
         sshagent(credentials: ['71a7ee52-1c6a-476a-860f-6070ab4eb875']) {
           sh './deploy.sh'
         }
       }
     }
  }

  post {
     success {
       slackSend(channel: '#test-dev', message: "Todo bien")
     }
     failure {
       slackSend(channel: '#test-dev', message: "Algo anda mal")
     }
  }
}