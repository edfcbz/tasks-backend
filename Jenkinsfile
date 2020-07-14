pipeline{
    agent any
    stages{

        stage ('Test Environment: Backend Building'){
            steps{
                bat 'mvn clean package -DskipTests=true'
            }
        }  

        stage ('Test Environment: Testing Backend by Unit tests'){   
            steps{
                bat 'mvn test'
            }       
        }

        stage ('Test Environment: Pulling and building Frontend'){
            steps{
                dir('tasks-frontend'){
                    git credentialsId: 'github_login', url: 'https://github.com/edfcbz/tasks-frontend'
                    bat 'mvn package'
                }
            }
        } 

        stage('Test Environment: Creating and running by docker-compose'){
            steps{
                    bat 'docker-compose build'
                    bat 'docker-compose up -d'
            }
        }

        stage ('Test Environment: Testing Backend quality code by Sonar Analysis'){
            environment{
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps{
                withSonarQubeEnv('SONAR_LOCAL'){
                    bat "${scannerHome}/bin/sonar-scanner -X -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://192.168.99.100:9000 -Dsonar.login=09d7564c765c3ed85381b23df391613cce09dfc9 -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**Application.java,**RootController.java"
                }
            }
        } 

        //ESTE STAGE NÃO É NECESSÁRIO, POIS AO EXECUTAR O "DOCKER-COMPOSE UP" DO AMBIENTE DE TESTES, NESTE SÃO INSTANCIADAS IMAGENS DO TOMCAT DE FROTEND E BACKEND COM OS .WAR CORRESPONDENTES
        //ESTANDO ASSIM OS CONTAINNERS DISPONÍVEIS NO ENDEREÇO 192.168.99.100:8001 E 192.168.99.100:8002 (FRONTEND E BACKEND RESPECTIVAMENTE)
        //stage ('Test Environment: Backend Deploy'){
        //    steps{
        //        deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://192.168.99.100:8001')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
        //    }
        //} 

        stage('Test Environment: Pull API Tests Repository') {
            steps{
             dir('api-test'){
                    git credentialsId: 'github_login', url: 'https://github.com/edfcbz/tasks-api-test'
                }
            }
        } 

        stage('Test Environment: Running API Tests') {
            steps{
                dir('api-test'){
                    bat 'mvn test'
                }
            }
        } 



    }
}

