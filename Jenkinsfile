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
            steps{
                script{
                    withSonarQubeEnv('sonar7.6'){
                        sh 'mvn sonar:sonar'
                    }
                    def qg=waitforQualityGate()
                    if (qg.status != 'ok'){
                       error "pipeline aborted due to Quality Gate failure: ${qg.status}"
                    }
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