pipeline {
    agent any

    environment {
        JAVA_HOME = tool name: 'java 17 mounted docker volume', type: 'jdk'
    }

    triggers {
        pollSCM '* * * * *'
    }

    stages {       
        stage('Build') {
            steps {
                sh './mvnw clean package -Dspring.profiles.active=dev -V  -Dsurefire.useFile=false'
            }
        }
        post {
                always {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
        }
    }
}
