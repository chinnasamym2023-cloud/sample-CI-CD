pipeline { 
    agent any 
 
    environment { 
        DOCKER_IMAGE = "chinnasamy23mis0481/html-demo" 
    } 
 
    stages { 
 
        stage('Clone Code') { 
            steps { 
                git branch: 'main', 
                    url: 'https://github.com/chinnasamym2023-cloud/sample-CI-CD.git'
            } 
        } 
 
        stage('Build Docker Image') { 
            steps { 
                bat 'docker build -t %DOCKER_IMAGE% .' 
            } 
        } 
 
        stage('Push Image') { 
            steps { 
                withCredentials([usernamePassword(credentialsId: 'D1', 
                    usernameVariable: 'USER', passwordVariable: 'PASS')]) { 
                    bat 'docker login -u %USER% -p %PASS%' 
                    bat 'docker push %DOCKER_IMAGE%' 
                } 
            } 
        } 
 
        stage('Deploy to Kubernetes') { 
            steps { 
                withCredentials([file(credentialsId: 'K1', variable: 'KUBECONFIG')]) { 
                    bat '''
                        echo Checking kubectl...
                        kubectl version
                        echo Checking deployment file...
                        dir deployment.yaml
                        echo Deploying...
                        kubectl apply -f deployment.yaml --validate=false
                    '''
                } 
            } 
        } 
    }
}