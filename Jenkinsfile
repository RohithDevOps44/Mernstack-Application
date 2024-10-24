pipeline {
    agent any
    
     environment {
        ANSIBLE_PLAYBOOK = 'install_trivy_docker.yml'
        INVENTORY_FILE = 'inventory.ini'
        DOCKER_HUB_REPO_1 = 'rohithdevops44/test-repo'
        DOCKER_HUB_REPO_2 = 'rohithdevops44/test-repo-101'
      }
    stages {
        stage('Git Checkout Stage') {
            steps {
                git 'https://github.com/RohithDevOps44/Mernstack-Application.git'
            }
        }
        stage('Run Ansible Playbook') {
            steps {
                // Run the Ansible playbook
                sh "cd ansible/ && ansible-playbook -i ${INVENTORY_FILE} ${ANSIBLE_PLAYBOOK}"
            }
        }
        stage('Trivy Scan') {
            steps {
                sh "trivy fs --format table -o fs.html ."
            }
        }
        stage('Docker build and tag') {
            steps {
                script{ 
                         sh "cd app/backend/ && docker build ."
                         sh "docker tag mern-backend-${DOCKER_HUB_REPO_1}:${env.BUILD_NUMBER}"

                         sh "cd app/frontend/ && docker build ."
                         sh "docker tag mern-frontend-${DOCKER_HUB_REPO_2}:${env.BUILD_NUMBER}"
                   } 
            }
        }
    }
       stage('Docker Push') {
            steps {
                script{ 
                   	withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker'){ 
                        sh "docker push ${DOCKER_HUB_REPO_1}:${env.BUILD_NUMBER}" 
                        sh "docker push ${DOCKER_HUB_REPO_2}:${env.BUILD_NUMBER}"
                   } 
            }
        }
    }
}

