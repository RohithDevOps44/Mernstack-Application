pipeline {
    agent any
    
     environment {
        ANSIBLE_PLAYBOOK = 'install_trivy_docker.yml'
        INVENTORY_FILE = 'inventory.ini'
        DOCKER_HUB_REPO_1 = 'rohithdevops44/test-repo'
        DOCKER_HUB_REPO_2 = ''
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
                sh 'sudo apt update'
                sh 'sudo apt install -y ansible'
            }
        }
        stage('Run Ansible Playbook') {
            steps {
                // Run the Ansible playbook
                dir('/var/jenkins_home/workspace/${env.JOB_NAME}/ansibe')
                sh "ansible-playbook -i ${INVENTORY_FILE} ${ANSIBLE_PLAYBOOK}"
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
                         sh "cd app/backend/"
                         sh "docker build ."
                         sh "docker tag mern-backend ${DOCKER_HUB_REPO_1}:${env.BUILD_NUMBER}"

                         sh "cd app/frontend/"
                         sh "docker build ."
                         sh "docker tag mern-frontend ${DOCKER_HUB_REPO_2}:${env.BUILD_NUMBER}"
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

