pipeline{
    agent any
    stages{
        stage ('Build Backend'){
            steps{
                bat 'mvn clean package -DskipTests=true'
            }
        }
        stage ('Unit Test'){
            steps{
                bat 'echo meio'
                bat 'echo meio de novo'
            }
        }
    }
}