pipeline {

    agent any 

    trigger {
        pollSCM('* * * * *')
    }


    environment {
        SONAR_TOKEN = credentials('sonar_id')
        
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Nagendra1181/spring-petclinic.git'
            }
        }

        stage('Build Application') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarCloud Scan') {
            steps {
                sh """
                mvn sonar:sonar \
                -Dsonar.projectKey=Nagendra1181_spring-petclinic \
                -Dsonar.organization=Nagendra1181 \
                -Dsonar.host.url=https://sonarcloud.io/ \
                -Dsonar.login=$SONAR_id
                """
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}