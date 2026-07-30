pipeline {
    agent { label 'prod' }

    stages {
        stage('Pull SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/NizarSSaleh/simple-apps.git'
            }
        }
        
        stage('Build') {
            steps {
                sh'''
                cd apps
                npm install
                '''
            }
        }
        
        stage('Testing') {
            steps {
                sh'''
                cd apps
                npm test
                npm run test:coverage
                '''
            }
        }
        
        stage('Code Review') {
            steps {
                sh'''
                cd apps
		sonar-scanner \
  	  		-Dsonar.projectKey=simple-apps \
  			-Dsonar.sources=. \
  			-Dsonar.host.url=http://172.23.6.128:9000 \
  			-Dsonar.login=sqp_dec951de8a359c746db45394cddbc481350727c5
                '''
            }
        }
        
        stage('Deploy') {
            steps {
                sh'''
                docker compose up --build -d
                '''
            }
        }
        
        stage('Backup') {
            steps {
                 sh 'docker compose push' 
            }
        }
    }
}
