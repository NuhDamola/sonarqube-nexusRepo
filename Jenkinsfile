pipeline {
    agent any

    stages {   
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
                  withSonarQubeEnv('sonar_scanner') {
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
            steps {
               nexusArtifactUploader artifacts: [[artifactId: 'SampleWebApp', classifier: '', file: 'SampleWebApp/target/SampleWebApp.war', type: 'war']], credentialsId: 'nexuspassword', groupId: 'SampleWebApp', nexusUrl: 'ec2-54-88-248-61.compute-1.amazonaws.com:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'http://ec2-54-88-248-61.compute-1.amazonaws.com:8081/repository/maven-snapshots/', version: '1.0-SNAPSHOTS''
            }   
            
        }
        
        stage('deploy to tomcat') {
          steps {
              deploy adapters: [tomcat9(credentialsId: 'tomcatpassword', path: '', url: 'http://54.209.130.211:8080/')], contextPath: 'myapp', war: '**/*.war'
             
              
              
          }
            
        }
            
        }
} 
