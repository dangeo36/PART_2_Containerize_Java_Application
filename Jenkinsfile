pipeline {
    agent any
    tools{
        maven 'MAVEN3'
    }
    environment {
                AWS_ACCESS_KEY_ID = credentials('aws-creds')
                AWS_SECRET_ACCESS_KEY = credentials('aws-creds')
                SONAR_TOKEN = credentials('sonarcloud-creds')
            }
    stages {
        stage ('Sonarcloud scan') {
            steps {
                script {
                    echo 'scanning'
                    env.SONAR_TOKEN = "${SONAR_TOKEN}"
                    sh 'mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -X -Dsonar.projectKey=dangeo36_PART_2_Containerize_Java_Application'
                }
            }  
        }
        stage ('Build and Package Jar') {
            steps {
                script {
                    echo 'Building jar'
                    sh 'mvn clean package -DskipTests=true'
                }
            }  
        }
        stage ('Build docker image') {
            steps {
                script {
                    echo 'Building docker image'
                            sh "docker build -t dangeo36/part2_petclinic:$BUILD_NUMBER ."
                     }
                }
        }  


        stage ('Push to dockerhub') {
            steps {
                script {
                    echo 'Pushing to dockerhub'
                         withCredentials([usernamePassword(credentialsId: 'docker-creds', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                            sh "echo $PASS | docker login -u $USER --password-stdin"
                            sh "docker push dangeo36/part2_petclinic:$BUILD_NUMBER"
                      }

                }
            }  
        }
        
    }
}
