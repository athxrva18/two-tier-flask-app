
pipeline{
    
    agent { label "dev"};
    
    stages{
        stage("Code Clone"){
            steps{
               
                   git url: "https://github.com/athxrva18/two-tier-flask-app.git", branch: "master"
               
            }
        }
         
        stage("trivy file system scan"){
            steps{
                sh "trivy fs . -o results.json"
            }
        }
        
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
            
        }
        stage("Test"){
            steps{
                echo "Developer / Tester tests likh ke dega..."
            }
            
        }
        stage("Push to Docker Hub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCreds",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                    }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }
post{
    success{
        script{
            emailext from: 'atharva.deshmukh1804@gmail.com',
            to:'atharva.deshmukh1804@gmail.com',
            body: 'build success for demo CI-CD app',
            subject : 'Jenkins build success'
        }
    }
    failure{
        script{
            emailext from: 'atharva.deshmukh1804@gmail.com',
            to:'atharva.deshmukh1804@gmail.com',
            body: 'build failed for demo CI-CD app',
            subject : 'Jenkins build failed'
        }
    }
}
}
