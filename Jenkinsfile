pipeline{
    agent any
    stages{

        stage('Test: Pull configurations docker repository test') {
            steps{
             dir('devops'){
                    git credentialsId: 'github_login', url: 'https://github.com/edfcbz/devops'
                }
            }
        } 

        stage('Test: Pull Backend Repository') {
            steps{
             dir('backend-test'){
                    git credentialsId: 'github_login', url: 'https://github.com/edfcbz/tasks-backend'
                }
            }
        } 

        stage ('Test: Backend Building'){
            steps{
                dir('backend-test'){
                    bat 'mvn clean package -DskipTests=true'
                }
            }
        }  

        stage ('Test: Testing Backend by Unit tests'){   
            steps{
                dir('backend-test'){
                    bat 'mvn test'
                }
            }       
        }


        stage('Test: Creating and running Environment by docker-compose'){
            steps{
                dir('Pipeline'){
                    bat 'docker-compose build'
                    bat 'docker-compose up -d'
                }
            }
        }

        stage ('Test: Testing Backend code quality by Sonar Analysis'){
            environment{
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps{
                sleep(30)
                dir('backend-test'){
                    withSonarQubeEnv('SONAR_LOCAL'){
                        bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://192.168.99.100:9000 -Dsonar.login=30db8d152582d074103e10294ab19916daf8546e -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**Application.java,**RootController.java"
                    }
                }
            }
        } 

        stage ('Test: Testing Backend code quality by Quality Gate'){
            steps{
                dir('backend-test'){
                    sleep(5)
                    timeout(time: 1, unit: 'MINUTES'){
                        waitForQualityGate abortPipeline: true
                    }
                }
            }
        } 

        stage ('Test: Backend Deploy'){
            steps{
                dir('backend-test'){
                    deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://192.168.99.100:8002')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
                }
            }
        } 

        stage('Test: Pull API Tests Repository') {
            steps{
             dir('api-test'){
                    git credentialsId: 'github_login', url: 'https://github.com/edfcbz/tasks-api-test'
                }
            }
        } 

        stage('Test: Running API Tests') {
            steps{
                dir('api-test'){
                    bat 'mvn test'
                }
            }
        } 

        stage ('Test: Pulling Frontend'){
            steps{
                dir('frontend'){
                    git credentialsId: 'github_login', url: 'https://github.com/edfcbz/tasks-frontend'
                }
            }
        }  

        stage ('Test: Building Frontend'){
            steps{
                dir('frontend'){
                    bat 'mvn clean package'
                }
            }
        } 

        stage ('Test: Deploying Frontend'){
            steps{
                dir('frontend'){
                    deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://192.168.99.100:8001')], contextPath: 'tasks', war: 'target/tasks.war'
                }
            }
        } 

        stage('Test: Pull Funcional Test repository') {
            steps{
                dir('functional-test'){
                    git credentialsId: 'github_login', url: 'https://github.com/edfcbz/tasks-functional-test'
                }
            }
        }

        stage('Test: Running Funcional Test') {
            steps{
                dir('functional-test'){
                    sleep(5)
                    bat 'mvn test'
                }
            }
        }        

        stage('Production: Building by docker-compose'){
            steps{
                sleep(5)
                bat 'docker-compose build'
            }
        }

        stage('Production: Starting environment by running docker-compose up '){
            steps{
                sleep(5)
                bat 'docker-compose up -d'
            }
        }        	

        stage('Production: Fontend Health Check') {
            steps{
                sleep(5)
                dir('functional-test'){
                    bat 'mvn verify -Dskip.surefire.tests'
                }
            }
        }        	

    }
}

