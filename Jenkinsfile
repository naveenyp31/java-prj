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
                sh 'scp -o StrictHostKeyChecking=no target/myweb-0.0.1.war ec2-user@10.0.0.126:/home/ec2-user' 
            }
        }
    }
}
