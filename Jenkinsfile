pipeline {
    agent any 
    environment {
       DOCKERHUB_PASS =  "9650291450"
    }

    stages { 
        stage("Building The Survey Page") {
            steps {
                script {
                    checkout scm
                    sh 'rm -rf target/*.war'
                    sh 'jar -cvf target/swe645.war -C src/main/webapp/ .'
                    sh "docker login -u arajput4 -p ${DOCKERHUB_PASS}"
                    def customImage = docker.build("arajput4/surveyform:latest")
                }
            }
        }
        
        stage("Pushing the Image to DockerHub") {
            steps {
                script {
                    sh 'docker push arajput4/surveyform:latest'
                }
            }
        }
        
        stage("Deploy on Rancher as single pod") {
            steps {
                script {
                    sh 'kubectl set image deployment/deploy1 container-0=arajput4/surveyform:latest -n swe'
                }
            }
        }
        

        stage("Deploy on Rancher with load balancer") {
            steps {
                script {
                    sh 'kubectl set image deployment/lb1 container-0=arajput4/surveyform:latest -n swe'
                }
            }
        }
        
        
        

                
        
    }

}
