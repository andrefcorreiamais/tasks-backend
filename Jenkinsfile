pipeline {
    agent any 
    stages {
        stage ('Build Backend') {
            steps {
                bat 'mvn clean package -DskipTests=true'
            } 
        }
        stage ('Unit Tests') {
            steps {
                bat 'mvn test'
            } 
        }
        stage ('Sonar Analysis') {
            enviroment {
                scannerHome = toll 'SONAR_SCANNER'
            }
            steps {
                    withSonarQubeEnv('SONAR_LOCAL') {
                    bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=c0b396a1ac59ef7de7e20f8ad7c09f7eb52c1ce7 -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**Application.java"
                    //bat "\"${scannerHome}/bin/sonar-scanner\" -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=819de48f677879d4e90027ee77c6db9a50096a63 -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=exclusions=**./.mvn/**,**/src/test**,**/model/**,**Application.java"
                    }
                } 
        }
   }
}


