// 4a37f4f08323805b85b935e0548d81d2cd6be150

// mvn sonar:sonar \
//   -Dsonar.projectKey=jenkinstest \
//   -Dsonar.host.url=http://54.243.26.167:9000 \
//   -Dsonar.login=4a37f4f08323805b85b935e0548d81d2cd6be150

pipeline {
    agent any
    tools { 
        maven 'maven-3.8.6' 
    }
    stages {
        stage('Checkout git') {
            steps {
               git branch: 'main', url: 'https://github.com/2jithin/sampletomcatapplication.git'
            }
        }
        
        stage ('Build & JUnit Test') {
            steps {
                sh 'mvn install -qa' 
            }
            post {
               success {
                    junit 'target/surefire-reports/**/*.xml'
                }   
            }
        }
        stage('SonarQube Analysis'){
            steps{
                   withSonarQubeEnv('sonarqube') {
                        sh 'mvn clean verify sonar:sonar \
                        -Dsonar.projectKey=devops-project-key \
                        -Dsonar.host.url=$sonarurl \
                        -Dsonar.login=$sonarlogin'
                    }
            }
        }
        stage('Deploy'){
            steps{
                sh 'timeout 60s java -jar /var/lib/jenkins/workspace/devopscicd/target/demo-0.0.1-SNAPSHOT.jar'
            }
        }
        }
    }
        