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
        stage("Quality Gate"){
            timeout(time: 1, unit: 'HOURS') {
                def qg = waitForQualityGate()
                if (qg.status != 'OK') {
                    error "Pipeline aborted due to quality gate failure: ${qg.status}"
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
    }
}
