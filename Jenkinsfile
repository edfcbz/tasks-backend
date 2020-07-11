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

        stage ('Test Environment: Testing Backend code quality by Sonar Analysis'){
            environment{
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps{
                withSonarQubeEnv('SONAR_LOCAL'){
                    bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://192.168.99.100:9000 -Dsonar.login=30db8d152582d074103e10294ab19916daf8546e -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**Application.java,**RootController.java"
                }
            }
        } 

        stage ('Test Environment: Testing Backend code quality by Quality Gate'){
            steps{
                sleep(5)
                timeout(time: 1, unit: 'MINUTES'){
                    waitForQualityGate abortPipeline: true
                }
            }
        } 

        stage ('Test Environment: Backend Deploy'){
            steps{
                deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://192.168.0.105:8001')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        } 

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

        stage ('Test Environment: Pulling Frontend'){
            steps{
                dir('frontend'){
                    git credentialsId: 'github_login', url: 'https://github.com/edfcbz/tasks-frontend'
                }
            }
        }  

        stage ('Test Environment: Building Frontend'){
            steps{
                dir('frontend'){
                    bat 'mvn clean package'
                }
            }
        } 

        stage ('Test Environment: Deploying Frontend'){
            steps{
                dir('frontend'){
                    deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://192.168.0.105:8001')], contextPath: 'tasks', war: 'target/tasks.war'
                }
            }
        } 

        stage('Test Environment: Pull Funcional Test repository') {
            steps{
                dir('functional-test'){
                    git credentialsId: 'github_login', url: 'https://github.com/edfcbz/tasks-functional-test'
                }
            }
        }

        stage('Test Environment: Running Funcional Test') {
            steps{
                dir('functional-test'){
                    sleep(8)
                    bat 'mvn test'
                }
            }
        }        

        stage('Production Environment: Building by docker-compose'){
            steps{
                bat 'docker-compose build'
            }
        }

        stage('Production Environment: running docker-compose up '){
            steps{
                bat 'docker-compose up -d'
            }
        }        	

        stage('Production Environment: Fontend Health Check') {
            steps{
                sleep(10)
                dir('functional-test'){
                    bat 'mvn verify -Dskip.surefire.tests'
                }
            }
        }        	

    }
}

