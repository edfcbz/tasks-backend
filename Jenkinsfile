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

        stage ('Test: Testing Backend code quality by Sonar Analysis'){
            environment{
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps{
                //sleep(30)
                withSonarQubeEnv('SONAR_LOCAL'){
                    bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://192.168.99.100:9000 -Dsonar.login=09d7564c765c3ed85381b23df391613cce09dfc9 -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**Application.java,**RootController.java"
                }
            }
        } 


    }
}

