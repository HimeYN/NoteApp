pipeline {
  agent any
  
  stages {
    stage('Checkout') {
      steps {
        // Récupère les playbooks Ansible depuis le dépôt Git
        checkout([$class: 'GitSCM', 
          branches: [[name: 'Production']], 
          doGenerateSubmoduleConfigurations: false, 
          extensions: [], 
          submoduleCfg: [], 
          userRemoteConfigs: [[url: 'git@github.com:HimeYN/NoteApp.git']]
        ])
      }
    }   

    stage('Deploiement') {
      agent { node { label 'ansible'} }
      environment {
        // Définit les variables d'environnement pour l'utilisateur distant et les informations d'authentification SSH
        remoteUser = 'ubuntu'
        sshKey = credentials('081f3cd7-f557-438a-9c4c-acf8760d8d1f')
      }      
      steps {
        // Exécute les commandes Ansible pour déployer les playbooks sur l'agent distant
        withEnv(["ANSIBLE_CONFIG=ansible/ansible.cfg"]) {
          sh "ansible-playbook -i ansible/inventory ansible/docker.yml"
        }
      }
    }
  }
}
