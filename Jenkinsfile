pipeline{
    agent any
    stages{
        stage ('Build Backend'){
            steps{
                bat 'mvn clean package -DskipTests=true'
            }
        }
        stage ('Unit Tests'){
            steps{
                bat 'mvn test'
            }
        }
        stage ('Sonar Analysis'){
            environment{
                scannerHome = tool 'SONAR_SCANNER'
            } 
            steps{
                bat "echo ver aula de instalaççao do sonar e configuração estática Pipeline 58"
            }
        }  
        stage ('Quality gate'){
            steps{
                bat "echo ver aula de instalaçao do QUALITY GATE e configuração estática Pipeline 59"
            }
        }

        stage ('Deploy Backend'){
            steps{
                deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        }

        stage ('API Tests'){
            steps{
                dir('api-test'){
                    git credentialsId: 'github_login', url: 'https://github.com/edfcbz/tasks-api-test'
                    bat 'mvn test'
                }
            }
        }

    }               
    
}

