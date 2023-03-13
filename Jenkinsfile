pipeline{
    agent any 
    environment{
        docker_psw = credentials('docker-password')
    }
    stages{
        stage('Sonar-Scan'){
            agent{
                docker{
                    image 'maven'
                    args '-v /root/.m2:/root/.m2'
                }
            }

            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-jenkins'){
                
                        sh 'mvn clean deploy'
                        sh 'mvn sonar:sonar'
                    }
                    timeout(5){
                        def quality_gate = waitForQualityGate()
                        if (quality_gate != 'OK'){
                            error "sonar analysis failed"
                        }

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
