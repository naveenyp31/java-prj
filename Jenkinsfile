pipeline {
    agent any
    tools{
        maven ('maven381')

    }
    stages{
        stage('scm checkout'){
            steps{
                git credentialsId: 'Git_cred', 
                url: 'https://github.com/naveenyp31/java-prj.git'
            }
        }
        stage('Buld and test'){
            steps{
                sh 'mvn clean install'
            }
        }
        stage('code analsys'){
            steps{
                withSonarQubeEnv('sonar7.6') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('upload artifact to nexus'){
            steps{
            nexusArtifactUploader artifacts: 
            [
                [artifactId: 'myweb', 
                classifier: '', 
                file: 'target/myweb-0.0.1-SNAPSHOT.war', 
                type: 'war']
                ], 
                credentialsId: 'nexus-artfact', 
                groupId: 'in.javahome', 
                nexusUrl: 'http://15.207.87.57:8081/', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'webapp-snapshot', 
            version: '0.0.1-SNAPSHOT'
            }
        }
        stage('deploy to dev env'){
            steps{
                deploy adapters: [tomcat9(credentialsId: 'tomcat-cred', 
                path: '', 
                url: 'http://3.108.223.176:8080/')], 
                contextPath: null, war: '**/*.war'
            }
        }
    }
}