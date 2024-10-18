pipeline {
    agent any
    
     environment {
        ANSIBLE_PLAYBOOK = 'install_trivy_docker.yml'
        INVENTORY_FILE = 'inventory.ini'
        DOCKER_IMAGE_NAME = 'your-docker-image-name'
        DOCKER_HUB_REPO = 'rohithdevops44/test-repo'
      }
    stages {
        stage('Git Checkout Stage') {
            steps {
                git 'https://github.com/RohithDevOps44/Mernstack-Application.git'
            }
        }
        stage('Dependancy Check Using OWZAP') {
            steps {
                 dependencyCheck additionalArguments: ' --scan ./ ', odcInstallation: 'DC' 
                 dependencyCheckPublisher pattern: '**/dependency-check-report.xml' 
            }
        }
        stage('Install Ansible') {
            steps {
                // Install Ansible on the Jenkins agent
                sh 'sudo apt-get update'
                sh 'sudo apt-get install -y ansible'
            }
        }
        stage('Run Ansible Playbook') {
            steps {
                // Run the Ansible playbook
                sh "cd /ansibe/ansible-playbook -i ${INVENTORY_FILE} ${ANSIBLE_PLAYBOOK}"
            }
        }
        stage('Trivy Scan') {
            steps {
                sh "trivy fs ."
            }
        }
        stage('Docker build and tag') {
            steps {
                script{ 
                         docker.build("${DOCKER_IMAGE_NAME}") 
                         sh "docker tag ${DOCKER_IMAGE_NAME} ${DOCKER_HUB_REPO}:${env.BUILD_NUMBER}"
                   } 
            }
        }
    }
       stage('Docker Push') {
            steps {
                script{ 
                   	withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker'){ 
                        sh "docker push ${DOCKER_HUB_REPO}:${env.BUILD_NUMBER}" 
                   } 
            }
        }
    }
}

