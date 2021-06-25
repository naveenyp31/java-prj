pipeline{
    agent any
    tools{
        maven 'maven381'
    }
    stages{
        stage('checkout scm'){
            steps{
                git credentialsId: 'Git_cred', 
                url: 'https://github.com/naveenyp31/java-prj.git'
            } 
        }
        stage('Quality Gate status check') {
            steps {
                withSonarQubeEnv('sonar7.6') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Build'){
            steps{
                sh "mvn clean install -DskipTests"
            }
        }
        stage('unit Test'){
            steps{
                sh "mvn test"
            }
        }
        stage('deploy to Nexus repo'){
            steps{
        
            }
        }
        stage('deploy to dev env'){
            steps{
                deploy adapters: [tomcat9(credentialsId: 'tomcat-cred', 
                path: '', 
                url: 'http://3.108.223.176:8080')], 
                contextPath: null, war: '**/*.war'
            }
        }
    }
}
