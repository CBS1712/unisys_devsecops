pipeline {
    agent any
    stages {
        // create stage 1 for fetching git repo info
        stage('fetching git repo details'){
            // steps
            steps {
                echo 'fetching git repo'
                git branch: 'springboot', url:'https://github.com/CBS1712/unisys_devsecops.git'
                sh 'ls'
            }
            
        }
        // creating second stage for SAST analysis for any bugs

        stage ('SAST using trivy for critical vulnerabilities'){
          steps
          {
            echo 'using trivy to scan code pushed by developers'
            sh 'trivy fs --scanners vuln,secret,misconfig .'
          }
        }
        

      // using compsoe
        stage('compsoe for build and curl for app status test') {
            steps {
                echo 'running docker compose'
                sh 'docker-compose down'
                sh 'docker-compose up -d --build'
                sh 'docker-compose ps'                
            }
            
        }
        
        stage('building image and pushing it') {
            steps {
                echo 'using docker pipeline plugin to build and push image'
                script {
                    def imageName = "chetan1712/cbsjava"
                    def imageTag  = "tomcat$BUILD_NUMBER"
                    def cbsCred = "480087f2-27c4-4acf-b208-19b1e0b0cf57"
                    // building image 
                    docker.build(imageName + ":" + imageTag , " -f Dockerfile .")
                    // pushing image 
                    docker.withRegistry('https://registry.hub.docker.com',cbsCred){
                        docker.image(imageName + ":" + imageTag).push()
                    }
                }
            }
            
        }
    }
}