pipeline{
    agent any
    environment {
        selenoid_conf_dest = "https://github.com/Evafan1337/selenoid_conf"
    }
    options{
        timestamps()
    }
    stages{
        stage("Get webapp src"){
            steps{
                sh """ 
                #!/bin/bash
                mkdir webapp && cd webapp
                git clone https://github.com/Evafan1337/908335-cinemaddict-14/
                """ 
                //git branch: 'master', url: 'https://github.com/Evafan1337/908335-cinemaddict-14/'
                
            }
        }
        stage("Install packages and build webapp via Docker"){
            steps{
                sh """ 
                #!/bin/bash
                cd webapp
                docker build -t js_app:latest ./908335-cinemaddict-14/
                docker run -d js_app
                """ 
            }
        }
        stage("Get selenoid configuration"){
            steps{
                echo "Start selenoid"
                sh 'git clone https://github.com/Evafan1337/selenoid_conf'
                sh 'ls'
                sh 'pwd'

            }
        }
        stage("Start selenoid"){
            steps{
                sh """ 
                #!/bin/bash
                cd selenoid_conf
                mkdir target
                pwd
                ls -l
                cd selenoid_config
                pwd
                ls -l
                cd ..
                cat docker-compose.yml
                docker-compose up
                """
                
                sh """
                docker ps -a
                docker inspect selenoid
                """
            }
        }
        stage("Get tests source code"){
            steps{
                sh """ 
                #!/bin/bash
                mkdir tests && cd tests
                git clone https://github.com/Evafan1337/908335-cinemaddict-14/
                """ 
                //git branch: 'selenoid', url: 'https://github.com/Evafan1337/cinemaddict_tests/'
            }
        }
        stage("Run tests"){
            steps{
                sh """ 
                #!/bin/bash
                pwd
                ls
                docker build -t behave .
                docker run behave
                """ 
            }
        }
        stage("Handle result"){
            steps{
                sh """ 
                #!/bin/bash
                pwd
                ls
                """ 
            }
        }
    }
    post{
        always{
            echo 'end!'
            sh 'rm -rf selenoid_conf/'
            sh 'rm -rf webapp/'
            
            sh 'docker stop selenoid-ui'
            sh 'docker stop selenoid'
            
            sh 'docker rm selenoid-ui'
            sh 'docker rm selenoid'
            
            sh 'docker container prune'
        }
    }
}
