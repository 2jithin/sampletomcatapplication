// 652d97c92761bbe219d1699c107860c0ba7417dd

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
                sh 'ls'
                //git branch: 'main', url: 'https://github.com/2jithin/sampletomcatapplication.git'
            }
        }
        
        stage ('Build & JUnit Test') {
            steps {
                sh 'mvn install -q' 
            }
            post {               
                success {
                    junit skipPublishingChecks: true, testResults: 'server/target/surefire-reports/**/*.xml' // due to [Checks API] No suitable checks publisher found. added this line of script
                }
                failure {
                    echo 'failued due to test result file missing'
                }  
            }
        }
        stage('SonarQube Analysis'){
            steps{
                   withSonarQubeEnv('sonarqube') {
                        sh 'mvn clean verify sonar:sonar -q \
                        -Dsonar.projectKey=devops-project-key \
                        -Dsonar.host.url=http://18.219.112.218:9000 \
                        -Dsonar.login=483e7f9d333cf43626b9eec4fc50533891f87b4e'
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
        
