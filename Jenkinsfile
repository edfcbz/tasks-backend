pipeline{
    agent any
    stages{

        stage ('Test: Backend Building'){
            steps{
                bat 'mvn clean package -DskipTests=true'
            }
        }  

        stage ('Test: Testing Backend by Unit tests'){   
            steps{
                bat 'mvn test'
            }       
        }

        stage ('Test: Pulling and building Frontend'){
            steps{
                dir('tasks-frontend'){
                    git credentialsId: 'github_login', url: 'https://github.com/edfcbz/tasks-frontend'
                    bat 'mvn package'
                }
            }
        } 

        stage('Test: Creating and running Environment by docker-compose'){
            steps{
                    bat 'docker-compose build'
                    bat 'docker-compose up -d'
            }
        }


    }
}

