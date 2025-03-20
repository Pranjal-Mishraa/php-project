pipeline {
    agent any
    stages {
        stage('Git Clone') {
            steps {
                git url: 'https://github.com/Pranjal-Mishraa/php-project.git', branch: 'master'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t pranjalmishraa541/akshatnewimg6july:v1 .'
                    sh 'docker images'
                }
            }
        }
        
        stage('Docker Login & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push pranjalmishraa541/akshatnewimg6july:v1'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    def dockerrm = 'sudo docker rm -f My-first-containe2211 || true'
                    def dockerCmd = 'sudo docker run -itd --name My-first-containe2211 -p 8083:80 pranjalmishraa541/akshatnewimg6july:v1'
                    
                    sshagent(['sshkeypair']) {
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.36.10 '${dockerrm}'"
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.36.10 '${dockerCmd}'"
                    }
                }
            }
        }
    }
}
