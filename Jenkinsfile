@Library("Shared") _
pipeline{
    
    agent { label "dev"};
    
    stages{
        stage("Code Clone"){
            steps{
               
                   script{
                       clone("https://github.com/athxrva18/two-tier-flask-app.git","master")
                   }
               
            }
        }
         
        stage("trivy file system scan"){
            steps{
                script{
                    trivy_fs()
                }
                    
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
                script{
                    docker_push("dockerHubCreds","two-tier-flask-app")
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
