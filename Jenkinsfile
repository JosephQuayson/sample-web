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
                        sh '''
                         mvn clean
                         mvn sonar:sonar
                        '''
                    }
                    timeout(5){              
                        def quality_gate = waitForQualityGate()
                        if (quality_gate.status != 'OK'){
                            error "sonar analysis failed"
                        }
                    }
                }
            }            
        }
        stage('build application'){
            agent{
                docker{
                    image 'maven'
                    args '-v /root/.m2:/root/.m2'
                }
            }
            steps{
                script{
                    sh "mvn clean deploy"
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
