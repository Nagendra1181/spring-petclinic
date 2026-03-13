pipeline {

    agent any 

    triggers {
        pollSCM ('* * * * *')
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
              withCredentials([string(credentialsId: 'sonar_id', variable: 'SONAR_TOKEN')]) {
                withSonarQubeEnv('Sonar') {
                  sh """
                   mvn sonar:sonar \
                  -Dsonar.projectKey=Nagendra1181_spring-petclinic \
                  -Dsonar.organization=Nagendra1181 \
                  -Dsonar.host.url=https://sonarcloud.io/ \
                  -Dsonar.login=$SONAR_TOKEN
                 """
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
    }
}