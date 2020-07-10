pipeline{
    agent any
    stages{
        stage ('Backend: Build'){
            steps{
                bat 'mvn clean package -DskipTests=true'
            }
        }  

        stage ('Unit tests'){            
            steps{
                bat 'mvn test'
            }
        }

        stage ('Sonar Analysis'){
            environment{
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps{
                withSonarQubeEnv('SONAR_LOCAL'){
                    bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://192.168.99.100:9000 -Dsonar.login=30db8d152582d074103e10294ab19916daf8546e -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**Application.java,**RootController.java"
                }
            }
        } 

        stage ('Quality Gate'){
            steps{
                sleep(5)
                timeout(time: 1, unit: 'MINUTES'){
                    waitForQualityGate abortPipeline: true
                }
            }
        } 

        stage ('Backend: Deploy'){
            steps{
                deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://192.168.0.105:8001')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        } 

        stage('Pull and run API Tests Repository') {
            steps{
                dir('api-test'){
                    git credentialsId: 'github_login', url: 'https://github.com/edfcbz/tasks-api-test'
                    bat 'mvn test'
                }
            }
        } 

        stage ('Frontend: Pull, Build and Deploy'){
            steps{
                dir('frontend'){
                    git credentialsId: 'github_login', url: 'https://github.com/edfcbz/tasks-frontend'
                    bat 'mvn clean package'
                    deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://192.168.0.105:8001')], contextPath: 'tasks', war: 'target/tasks.war'
                }
            }
        }  

        stage('Fontend: Functional Tests') {
            steps{
                dir('functional-test'){
                    git credentialsId: 'github_login', url: 'https://github.com/edfcbz/tasks-functional-test'
                    bat 'mvn test'
                }
            }
        } 

    }
}

