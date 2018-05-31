pipeline {
    agent any
   
    stages {
        stage("checkout"){
            steps {
                git 'https://github.com/pavanhub52/docker-project2.git' 
            }
        }

        stage("static code analysis"){
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh '/opt/sonar/bin/sonar-scanner -Dsonar.projectKey=ZervOnboarding -Dsonar.sources=api'
                }
            }
        }

        stage("build docker image"){
            steps {
                sh "docker-compose build"
            }
        }


        stage("env cleanup"){
            steps {
                sh "docker image prune -f"
            }
        }

        stage("Launch service"){
            steps {
                sh "docker-compose stop"
                sh "docker-compose up -d"
		sh "docker logs zervonboarding_onboarding_1"
		sh "docker logs zervonboarding_mongo_1"
            }
        }

        stage("Launch Info"){
            steps {
                echo "service running on http://${ip}:7360"
            }
        }
	
        stage("Robot testing") {
            steps {
                dir( "testing/" ) {
                    sh "docker build -t robot ."
                    withDockerContainer(args: '-v $PWD/testing:/app', image: 'robot') {
                    sh "robot Onboarding.robot"
                    } 			 			
                }		
            }
        }	
    }
		
  /*  post {
        always {
            node("build") {
               dir ( "/home/ubuntu/workspace/ZervOnboarding/testing/" ) { 
                    step([
                            $class: 'RobotPublisher',
                            disableArchiveOutput: false,
                            logFileName: "log.html",
                            outputPath: '.',
                            outputFileName: "*.xml",
                            passThreshold: 0,
                            unstableThreshold: 0,
                            otherFiles: " ",
                            reportFileName: "report.html",
                   ])
                }
            }
        }    
    } */
}
