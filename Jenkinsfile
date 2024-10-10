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
        
    }
}