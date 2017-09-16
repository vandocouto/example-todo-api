node('php'){
    stage('Clean'){
        deleteDir()
        sh 'ls -la'
    }
    
    stage('Fetch') {
        checkout scm
    }
    
    stage('Build'){
        sh 'composer install --prefer-dist --no-dev --ignore-platform-reqs'
        sh 'php artisan config:cache'
        // sh 'php artisan route:cache'
    }
    
    stage('Docker Build') {
        sh 'docker build -t vandocouto/todoapi:$BUILD_NUMBER .'
    }
    
    stage('Docker Ship') {
        withCredentials([string(credentialsId: 'HUB', variable: 'HUB')]) {     
            sh  'sudo docker login -u churrops -p $HUB hub.docker.com'
            sh  'docker push vandocouto/todoapi:$BUILD_NUMBER'
        }
    }
}
