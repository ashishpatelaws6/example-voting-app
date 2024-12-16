pipeline{
    agent {
        label 'worker'
    }
    stages{
        stage("Build and Push"){
            steps{
                sh '''
                     cd vote
                     aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 796973483974.dkr.ecr.ap-south-1.amazonaws.com
                     docker build -t 796973483974.dkr.ecr.ap-south-1.amazonaws.com/ap-ecr:vote-v${BUILD_NUMBER} .
                     docker push 796973483974.dkr.ecr.ap-south-1.amazonaws.com/ap-ecr:vote-v${BUILD_NUMBER}
                '''
            }
        }
        stage("Deploy"){
            steps{
                sh '''
                kubectl  set image deployment vote vote=796973483974.dkr.ecr.ap-south-1.amazonaws.com/ap-ecr:vote-v${BUILD_NUMBER}
                kubectl rollout restart deployment vote
                '''
            }
        }
    }
}
