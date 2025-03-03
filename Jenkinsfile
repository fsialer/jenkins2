pipeline {
   //agent { label 'agente1' } //se coloca el agente que qconfiguraste en jenkins
   agent any

   environment{
    SONAR_TOKEN=credentials('Sonar')
   }

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
        withSonarQubeEnv('docker sonar'){
          script {
              docker.image('sonarsource/sonar-scanner-cli').inside('--network ci-network') {
                sh '''
                sonar-scanner \
                  -Dsonar.host.url=http://sonarqube:9000 \
                  -Dsonar.projectKey=my-php-app \
                  -Dsonar.sources=src \
                  -Dsonar.login=$SONAR_TOKEN \
                  -Dsonar.branch.name=$GIT_BRANCH
                '''
                }
          }
        }
       }
     }

     stage('Deploy'){
      steps{
        script{
          def qg=waitForQualityGate()
          if(qg.status!='OK'){
            error "PIPELINE ERROR! ${qg.status}"
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

    //  stage('Deploy') {
    //    steps {
    //      sshagent(credentials: ['71a7ee52-1c6a-476a-860f-6070ab4eb875']) {
    //        sh './deploy.sh'
    //      }
    //    }
    //  }
  }

  post {
     success {
       slackSend(channel: '#notifications-jenkins', message: "Todo bien")
     }
     failure {
       slackSend(channel: '#notifications-jenkins', message: "Algo anda mal")
     }
  }
}