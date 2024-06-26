pipeline {
    agent any
    
    environment {
        // Define the base URL for Jenkins
        JENKINS_URL = 'http://192.168.37.128:8080'
    }

    stages {
        stage('Checkout') {
            steps {
                // clean the directory
                sh "rm -rf *"
                // Checkout the Git repository
                sh "git clone https://github.com/simoks/java-maven.git"
            }
        }
        stage('Build') {
            steps {
                // Here, we can can run Maven commands
                script {
                    
                    def currentDir = pwd()
                    echo "Current directory: ${currentDir}"
                    
                    // Navigate to the directory containing the Maven project
                    dir('java-maven/maven') {
                        // Run Maven commands
                        sh 'mvn clean test package'
                        sh "java -jar target/maven-0.0.1-SNAPSHOT.jar"
                    }
                    
                   
                }
            }
        }
        
        stage('Archive') {
            steps {
                // Archive the JAR files generated by Maven
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
        
        stage('Notify Slack'){
            steps {
                script {
                    def artifactPath = "java-maven/maven/target/maven-0.0.1-SNAPSHOT.jar"
                    def artifactURL = "${env.JENKINS_URL}/job/${env.JOB_NAME}/${env.BUILD_NUMBER}/artifact/${artifactPath}"

                    //Add channel name
                    slackSend channel: 'mavenproject',
                    message: "IMAD SAMADI : Un nouveau build Java est disponible: ---> Resultat: ${currentBuild.currentResult}, Job: ${env.JOB_NAME}, Build: ${env.BUILD_NUMBER} \n <${artifactURL}|Cliquer ici pour télécharger>"
                }
            }
        }
    }
}
