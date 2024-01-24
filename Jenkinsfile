pipeline {
  agent {
    node {
      label 'workstation'
    }
  }

  options {
    ansiColor('xterm')
  }

  parameters {
    string(name: 'COMPONENT', defaultValue: '', description: 'Which Component')
    string(name: 'ENV', defaultValue: '', description: 'Which Env')
    string(name: 'APP_VERSION', defaultValue: 'Mr Jenkins', description: 'Which App_Version')
  }

  Stages {

    stage('Get Servers') {
      steps {
        sh 'aws ec2 describe-instances --filters "Name=tag:Name,Values=${component}-${env}" --query "Reservations[*].Instances[*].PrivateIpAddress"  --output text >inv'
      }
    }

    stage('Deploy Application') {
       steps {
         sh 'ansible-play -i inv main.yml -e role_name=${COMPONENT} -e env=${ENV} -e ansible_user=centos -e ansible_password=DevOps321'
       }
    }
  }
}

