pipeline {
    agent any

    stages {
        stage('Git checkout') {
            steps {
                git 'https://github.com/yomi-goke/infrastructureAsCode.git'
            }
        }
        
        stage('Build with maven') {
            steps {
                sh 'cd SampleWebApp && mvn clean install'
            }
        }
        
             stage('Test') {
            steps {
                sh 'cd SampleWebApp && mvn test'
            }
        
            }
        stage('Code Qualty Scan') {

           steps {
                  withSonarQubeEnv('sonar-server') {
             sh "mvn -f SampleWebApp/pom.xml sonar:sonar"      
               }
            }
       }
        stage('Quality Gate') {
          steps {
                 waitForQualityGate abortPipeline: true
              }
        }
        stage('push to nexus') {
            steps {nexusArtifactUploader artifacts: [[artifactId: 'SampleWebApp', classifier: '', file: 'SampleWebApp/target/SampleWebApp.war', type: 'war']], credentialsId: 'nexus', groupId: 'SampleWebApp', nexusUrl: '18.224.228.97:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0-SNAPSHOT'
           }
            
       } 
        stage('deploy to tomcat') {
        steps {
            deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://18.191.89.83:8080/')], contextPath: 'myapp', war: '"**/*.war"'
            
        }
            
       }
        
        }
            
 }

 
