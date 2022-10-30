pipeline {
    agent any 
    environment {
       DOCKERHUB_PASS =  "NE=.ce,c-bf@nS3"
    }

    stages { 
        stage("Building The Survey Page") {
            steps {
                script {
                    checkout scm
                    sh 'rm -rf target/*.war'
                    sh 'jar -cvf target/SurveyForm.war -C src/main/webapp/ .'
                    sh "docker login -u heysreenir -p ${DOCKERHUB_PASS}"
                    def customImage = docker.build("heysreenir/surveyform-assn2:latest")
                }
            }
        }
        
        stage("Pushing the Image to DockerHub") {
            steps {
                script {
                    sh 'docker push heysreenir/surveyform-assn2:latest'
                }
            }
        }
        
        stage("Deploy on Rancher as single pod") {
            steps {
                script {
                    sh 'kubectl set image deployment/deploy1 container-0=heysreenir/surveyform-assn2:latest -n swe'
                }
            }
        }
        

        stage("Deploy on Rancher with load balancer") {
            steps {
                script {
                    sh 'kubectl set image deployment/lb1 container-0=heysreenir/surveyform-assn2:latest -n swe'
                }
            }
        }
        
        
        

                
        
    }

}
