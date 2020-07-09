TasksApplication{
    agent any
    stages{

        stage ('Um teste'){
            steps{
                bat 'Teste'
            }
        }   
    }
}



/*        stage ('Unit Tests'){
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

        stage ('Deploy Frontend'){
            steps{
                dir('frontend'){
                    git credentialsId: 'github_login', url: 'https://github.com/edfcbz/tasks-frontend'     
                    bat 'mvn clean package'
                    deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war'
                }
            }
        }      

        stage ('Functional Tests'){
            steps{
                dir('functional-test'){
                    git credentialsId ('github_login', 'https://github.com/edfcbz/tasks-functional-tests')
                    bat 'mvn test'
                }
            }            
        }

        stage('Deploy Prod'){
            steps{
                bat 'docker-compose build'
                bat 'docker-compose up -d'
            }
        }

        stage ('Health Check Prod'){
            steps{
                sleep(5) {
                dir('functional-test'){
                    bat 'mvn verify -Dskip.surefire.tests'
                    }
                }
            }            
        }
    }
    post{
        junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml, api-test/target/surefire-reports/*.xml, functional-test/target/surefire-reports/*.xml, functional-test/target/failsafe-reports/*.xml'  
    }    

*/