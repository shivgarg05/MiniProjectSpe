
pipeline{
    environment{
        docker_image = ""
        registryCredential = "dockerhub"
    }
    agent any
    stages{
        stage('Step 1: Git Clone'){
            steps{
                git branch: 'main', url: 'https://github.com/shivgarg05/MiniProjectSpe.git'
            }
        }
        stage('Step 2: Maven Build'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage('Step 3: Build Docker Image'){ 
            steps {
                script {
                    docker_image = docker.build "shivyanshgarg05/spe-mini-project:latest"
                }
            }
        }
        stage('Step 4: Push docker image to hub') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub') {
                        docker_image.push()
                    }
                }
            }
        }
        stage('Step 5: Ansible Deployment'){
            steps{
                sh 'pip install docker-py'
                ansiblePlaybook becomeUser: null, 
                colorized: true,
                credentialsId: 'docker',
                installation: 'Ansible',
                disableHostKeyChecking: true, 
                inventory: 'deployment/inventory', 
                playbook: 'deployment/deploy.yml', 
                sudoUser: null  
            }          
        }   
    }
     
}
