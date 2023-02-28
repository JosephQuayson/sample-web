def getDockerTag(){
    def tag = sh script: 'git rev-parse --short HEAD', returnStdout
    return tag
}

pipeline{
    agent any 
    environment{
        Docker_tag = getDockerTag()
    }
    stages{
        stage("sonar scan"){
                agent{
                    docker{
                        image 'maven'
                        // volume mounting 
                        args '-v /root/.m2 : /root/.m2'  
                    }
                }
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                           sh "printenv"
                           sh "mvn sonar:sonar" 
                    } 
                    timeout(5){
                        // Checking status of sonar analysis
                        // make sure to create a webhook inside sonarqube  
                        def quality_gate = waitForQualityGate()
                        if (quality_gate != 'OK'){
                            error "sonar analysis failed"
                        }
                    }
                }
            }
        }

        stage('bulid the application '){
            agent{
                docker{
                    image 'image'
                    args '-v /root/.m2:/root/.m2'
                }
            }

            steps{
                script{
                    sh "mvn clean deploy"
                }
            }

        }

        stage('docker build'){
            steps{
                script{
                    sh "docker build . -t  34.16.131.110:8083/sample-app:$Docker_tag"
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

