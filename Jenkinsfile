pipeline {
    agent any    
    stages {
        stage('Build') {
            steps {
        withCredentials([usernamePassword(credentialsId: 'jfrog-artifactory', usernameVariable: 'ARTIFACTORY_USER', passwordVariable: 'ARTIFACTORY_PASS')]) {
                bat 'mvn -B -DskipTests clean package -Dusername=$ARTIFACTORY_USER -Dpassword=$ARTIFACTORY_PASS'
        }
            }
        }
        stage('Test') {
            steps {
                bat 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
      stage('SonarQube'){
              steps {
                  withSonarQubeEnv('SonarQube'){
                      bat 'mvn clean package sonar:sonar'
                      echo 'Static Analysis Completed' 
                  }                
              }
      }
      stage('Build'){
              steps {
                bat 'copy %WORKSPACE%\target\*.jar C:\SWSetup\apache-tomcat-9.0.80-App\webapps'
                bat 'call C:\SWSetup\apache-tomcat-9.0.80-App\\bin\startup.bat' 
              }
      }      
    }
}
