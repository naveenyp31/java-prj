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
        stage('deploy to dev env'){
            steps{
                nexusArtifactUploader artifacts: [[artifactId: 'myweb', 
                classifier: '', 
                file: 'target/myweb-0.0.1.war', 
                type: 'war']], 
                credentialsId: 'nexus-artfact', 
                groupId: 'in.javahome', 
                nexusUrl: '10.0.0.126', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'http://3.6.91.236:8081/repository/myweb/', 
                version: '0.0.1' 
            }
        }
    }
}
