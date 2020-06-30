pipeline{
    agent any
    stages{
        stage ('Build Backend'){
            steps{
                bat 'mvn clean package -DskipTests=true'
            }
        }
        stage ('Meio'){
            steps{
                bat 'echo meio'
                bat 'echo meio de novo'
            }
        }
        stage ('Fim'){
            steps{
                sleep(5)
                bat 'echo fim'
            }
        }
    }
}