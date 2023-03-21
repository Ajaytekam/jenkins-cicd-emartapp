pipeline {
    agent any

    tools {
        maven "MAVEN3"
        jdk "OpenJDK8"
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Ajaytekam/jenkins-cicd-emartapp.git'
            }
        }

        stage('Unit Testing : SpringBoot API') {
            steps{
                sh 'mvn test -f javaapi/pom.xml'
            }
        }

        stage('Integration Testing: SpringBoot API') {
            steps{
                sh 'mvn verify -f javaapi/pom.xml'  
            }
        }

        stage('Maven Build: Build and Archieve artifacts and run junit tests') {
            steps {
                sh 'mvn clean install -f javaapi/pom.xml'
            }

            post {
                success {
                    echo 'Running Junit Test...' 
                    junit 'javaapi/target/surefire-reports/**/*.xml'
                    sh "test ${currentBuild.currentResult} != UNSTABLE" // workaround for UNSTABLE build mark
                    echo 'Now archiving it...'
                    archiveArtifacts artifacts: '**/javaapi/target/*.jar'
                }
            }
        }
        
        stage('Checkstyle Analysis') {
            steps {
                sh 'mvn checkstyle:checkstyle -f javaapi/pom.xml'
            }
        }



        /* 
        stage('SonarQube Code Analysis') {

            environment {
                scannerHome = tool 'Sonar4.8'
            }

            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-api') {
                        //sh 'mvn clean package sonar:sonar'
                        sh '''export JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64;
                        ${scannerHome}/bin/sonar-scanner -X \
                        -Dsonar.projectKey=vprofile \
                        -Dsonar.projectName=vprofile \
                        -Dsonar.projectVersion=1.0 \
                        -Dsonar.sources=src/ \
                        -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                        -Dsonar.junit.reportsPath=target/surefire-reports/ \
                        -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                        -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml
                       '''
                    }
                }
            }
        }

        stage('SonarQube Quality Gate Status') {
            steps{
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    //waitForQualityGate abortPipeline: true
                    waitForQualityGate abortPipeline: true, credentialsId: 'sonar-api'  
                }
            }
        }
        */

    }
}
