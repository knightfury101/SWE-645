pipeline {
    agent any 
    environment {
        DOCKERHUB_PASS =  credentials{'NE=.ce,c-bf@nS3'}
    }

    stages{ 
        stage{"Building The Survey Page"} {
            script {
                chechkout scm
                sh 'rm -rf *.war'
                sh 'jar -cvf StudentSurvey.war -C WebContent/ .'
                sh 'echo ${BUILD_TIMESTAMP}'
                sh "docker login -u heysreenir -p ${DOCKERHUB_PASS}"
                def customImage = docker.build{"heysreenir/surveyform-assn2:${BUILD_TIMESTAMP}"}
            }
        }
    

        stage{"Pushing Image to Dockerhub"}{
            steps{
                script{
                    sh 'docker push heysreenir/surveyform-assn2:${BUILD_TIMESTAMP}'
                }
            }
        }

        stage{"Deploying to Rancher as a single pod"}{
            steps{
                    sh 'kubectl set image deployment/swe645-assn2-deploy swe645-assn2-deploy=heysreenir/surveyform-assn2:${BUILD_TIMESTAMP} -n jenkins-pipeline'
                }
        }
        

        stage{"Deploying to Rancher as a loadbalancer"}{
            steps{
                    sh 'kubectl set image deployment/swe645-assn2-deploy-loadbalancer swe645-assn2-deploy-loadbalancer=heysreenir/surveyform-assn2:${BUILD_TIMESTAMP} -n jenkins-pipeline'
                }
        }   

        
    }

}