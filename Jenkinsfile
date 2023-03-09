
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
