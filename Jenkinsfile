#!/usr/bin/env groovy
pipeline {
    agent any

    environment {
        JAVA_HOME = tool name: 'java 17 mounted docker volume', type: 'jdk'
    }

    triggers {
        pollSCM '* * * * *'
    }

    stages {
        stage('Info') {
            steps {
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
                echo "JAVA_HOME is set to: $JAVA_HOME"
            }
        }

       // stage('Checkout') is implicit now by triggers

        stage('Build') {
            steps {
                sh './mvnw clean package -Dspring.profiles.active=dev -V  -Dsurefire.useFile=false'
            }
        }

        // post is out of stage("Build" now)
        post {
                always {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
        }
    }
}
