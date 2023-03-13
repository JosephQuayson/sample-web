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
                    withSonarQubeEnv(credentialsId: 'NEW-SONAR'){
                
                        sh 'mvn clean deploy'
                        sh 'mvn sonar:sonar'
                    }
                    timeout(5){
                        Checking status of sonar analysis
                        make sure to create a webhook inside sonarqube  
                        def quality_gate = waitForQualityGate()
                        if (quality_gate != 'OK'){
                            error "sonar analysis failed"
                        }

                    }
                }
            }
            
        }
    }
    // post{
    //     always{
    //         cleanWs()
    //     }
    // }
    
}
