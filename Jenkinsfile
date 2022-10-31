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
            environment {
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps {
                    withSonarQubeEnv('SONAR_LOCAL') {
                        bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=c0b396a1ac59ef7de7e20f8ad7c09f7eb52c1ce7 -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**Application.java"
                    }
                } 
        }
        stage ('Quality Gate') {
            steps {
                sleep(5)
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
                
            } 
        }
        stage ('Depoly Backend') {
            steps {
                    deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'                  
            }           
        }
        stage ('API Test') {
            steps {
                dir('api-test') {
                    git 'https://github.com/andrefcorreiamais/tasks-api-test'        
                    bat 'mvn test'
                }
            } 
        }  
        stage ('Depoly Frontend') {
            steps {
                dir('frontend') {
                    git 'https://github.com/andrefcorreiamais/tasks-frontend'        
                    bat 'mvn clean package'
                    deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war'                  
                }
            }           
        }
         stage ('Functional Test') {
            steps {
                dir('functional-test') {
                    git 'https://github.com/andrefcorreiamais/tasks-functional-tests'        
                    bat 'mvn test'
                }
            } 
        stage ('Deploy Prod') {
            steps {
                bat 'docker-compose build'
                bat 'docker-compose up'
                }
            }     
        }  
    }
  
}