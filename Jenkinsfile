pipeline {
    agent {
        label 'maven-agent'
    } 
    environment{
        MAVEN_HOME ='/usr/share/maven'
        TAG_NAME= "build-${new Date().format('yyyymmdd-HHmmss')}"
    }
    stages {
        stage('Check_out'){
            steps {
                script {
                    echo "Pulling the code from Github"
                }
                git branch:'main',
                url:'https://github.com/ManoharSankar/java_maven_app.git'
            }
        }
        stage('Parallel Build Stages') {
            parallel {
                stage('Build'){
                    steps {
                         script {
                            echo "Building the project"
                        }
                    sh 'mvn compile' 
                    }
                }
                stage('Test'){
                    steps {
                         script {
                            echo "Running the unit tests"
                        }
                    sh 'mvn test'
                    }
                }
            }
        }
                stage('Package'){
                    steps {
                        script {
                            echo "Packaging and the application"
                        }
                        sh 'mvn clean package install'
                    }
                }
                stage('Push artifact') {
                    steps {
                        script {
                            echo "Uploading the jar to nexus repo"
                        }
                    nexusArtifactUploader artifacts: [[artifactId: '01-maven-web-app', file: 'target/Maven-App-1.0.RELEASE.jar', type: 'jar']], credentialsId: 'nexus_Credentials', groupId: 'in.ashokit', nexusUrl: '15.207.113.98:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'ashokit-snapshot', version: '1.0-SNAPSHOT'
                    }
                }
    }
post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed! Check the logs for details.'
        }
        always {
            echo 'Performing final cleanup...'
        }
    }
}
