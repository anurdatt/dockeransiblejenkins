pipeline{
    agent {label 'linux'}
    tools {
      maven 'Maven3'
    }
    environment {
      DOCKER_TAG = getVersion()
    }
    stages{
//         stage('SCM'){
//             steps{
//                 git credentialsId: 'github', 
//                     url: 'https://github.com/anurdatt/dockeransiblejenkins'
//             }
//         }
        
        stage('Maven Build'){
            steps{
                sh "mvn clean package"
            }
        }
        
        stage('Docker Build'){
            steps{
                sh "docker build . -t anuran4u/hariapp:${DOCKER_TAG} "
            }
        }
        
        stage('DockerHub Push'){
            steps{
                withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerHubPwd')]) {
                    sh "docker login -u anuran4u -p $dockerHubPwd"
                }
                
                sh "docker push anuran4u/hariapp:${DOCKER_TAG} "
            }
        }
        
//         stage('Docker Deploy'){
//             steps{
//               ansiblePlaybook credentialsId: 'dev-server', disableHostKeyChecking: true, extras: "-e DOCKER_TAG=${DOCKER_TAG}", installation: 'ansible', inventory: 'dev.inv', playbook: 'deploy-docker.yml'
//             }
//         }
    }
}

def getVersion(){
    def commitHash = sh label: '', returnStdout: true, script: 'git rev-parse --short HEAD'
    return commitHash
}
