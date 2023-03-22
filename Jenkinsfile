pipeline {
    agent any

    environment {    
        registryCredential = 'ecr:ap-south-1:aws-creds'    
        appRegistry = "470960211354.dkr.ecr.ap-south-1.amazonaws.com/emartapp"
        vprofileRegistry = "https://470960211354.dkr.ecr.ap-south-1.amazonaws.com"  
    }   

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
        /*
        stage('Integration Testing: SpringBoot API') {
            steps{
                sh 'mvn verify -f javaapi/pom.xml'  
            }
        }
        */  
        stage('Maven Build: Build and Archieve artifacts and run junit tests') {
            steps {
                sh 'mvn clean install -f javaapi/pom.xml'
            }

            post {
                success {
                    echo 'Running Junit Test...' 
                    // junit 'javaapi/target/surefire-reports/*.xml' // if uncommented then shows release UNSTABLE
                    echo 'Now archiving it...'
                    archiveArtifacts artifacts: '**/javaapi/target/*.jar'
                }
            }
        }
        /*      
        stage('Checkstyle Analysis') {
            steps {
                sh 'mvn checkstyle:checkstyle -f javaapi/pom.xml'
            }
        }

        stage('SonarQube Code Analysis for SpringBoot') {

            environment {
                scannerHome = tool 'Sonar4.8'
            }

            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-api') {
                        //sh 'mvn clean package sonar:sonar'
                       sh '''export JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64;
                        ${scannerHome}/bin/sonar-scanner -X \
                        -Dsonar.projectKey=spring-boot-starter-parent \
                        -Dsonar.projectName=spring-boot-starter-parent \
                        -Dsonar.projectVersion=2.3.1 \
                        -Dsonar.sources=javaapi/src/ \
                        -Dsonar.java.binaries=javaapi/target/test-classes/com/springwork/bookwork/ \
                        -Dsonar.junit.reportsPath=javaapi/target/surefire-reports/ \
                        -Dsonar.java.checkstyle.reportPaths=javaapi/target/checkstyle-result.xml
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
    
        stage('Nexus Artifact uploader') {
            steps {
                script {

                    def version = sh script: 'mvn help:evaluate -Dexpression=project.version -q -DforceStdout -f javaapi/pom.xml', returnStdout: true
                    def artifactId = sh script: 'mvn help:evaluate -Dexpression=project.artifactId -q -DforceStdout -f javaapi/pom.xml', returnStdout: true
                    def packaging = sh script: 'mvn help:evaluate -Dexpression=project.packaging -q -DforceStdout -f javaapi/pom.xml', returnStdout: true
                    def groupId = sh script: 'mvn help:evaluate -Dexpression=project.groupId -q -DforceStdout -f javaapi/pom.xml', returnStdout: true
                    nexusArtifactUploader artifacts: [
                        [
                            artifactId: "${artifactId}", 
                            classifier: '', 
                            file: "javaapi/target/${artifactId}-${version}.jar",
                            type: "${packaging}"
                        ]
                    ], 
                    credentialsId: 'nexus-cred', 
                    groupId: "${groupId}",
                    nexusUrl: '172.31.37.118:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'emartapp-release', 
                    version: "${version}"
                }
            }
        }
        
        stage('Build App Image') {
            steps {
                script {
                    dockerImage = docker.build(appRegistry + ":$BUILD_NUMBER", "javaapi/.")
                }
            }
        }

        stage('Scan Image with trivy') {
            steps {
                sh 'trivy image --format template --template "@/usr/local/share/trivy/templates/html.tpl" -o report.html --timeout 30m ${appRegistry}:latest'
            }
        }
        
        stage('Upload app image') {
            steps {
                script {
                    docker.withRegistry( vprofileRegistry, registryCredential ) {
                        dockerImage.push("$BUILD_NUMBER")    
                        dockerImage.push("latest")   
                    }
                }
            }
        }
        */  


        stage('Build NodeJS App') {  
            steps {
                nodejs(nodeJSInstallationName: 'nodejs') {
                    sh 'cd nodeapi && npm install'
                    sh 'cd nodeapi && npm pack'
                }
            }
        }

    }
}
