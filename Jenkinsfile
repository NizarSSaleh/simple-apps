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
                    -Dsonar.projectKey=Simple-Apps \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=http://172.23.6.128:9000 \
                    -Dsonar.login=sqp_a85b30dd84469ce06d9d575487099e463fcad163
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
        
//        stage('Backup') {
//            steps {
//                 sh 'docker compose push' 
//            }
//        }
    }
}
