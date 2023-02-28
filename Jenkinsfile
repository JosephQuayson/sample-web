pipeline{
    agent any 
    stages{
        stage("sonar scan"){
                agent{
                    docker{
                        image 'maven'
                    }
                }
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-jenkins') {
                           sh "printenv"
                        
                           sh "mvn sonar:sonar" 
                    } 
                }
            }
        }
    }
    post{
        always{
            cleanWs()
        }
    }
}
