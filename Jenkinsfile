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
                sleep(5)
                timeout(time: 1, unit: 'MINUTES' ){
                waitForQualityGate abortPipeline = true
                }
            }
        }               
    }
}