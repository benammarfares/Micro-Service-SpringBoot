pipeline {
    agent {
        docker {
            image 'maven:3.9.2-amazoncorretto-20'
            args '-v $HOME/.m2:/root/.m2'
        }
    }
    stages {
        stage("Prepare Maven Repository") {
            steps {
                sh '''
                    if [  -d "$HOME/.m2/repository" ]; then
                        sudo chmod -R 777 "$HOME/.m2/repository"
                    fi
                '''
            }
        }
        stage("Build & SonarQube Analysis") {
            steps {
                dir('assurance') {
                    withSonarQubeEnv('sonarserver') {
                        echo 'Building the project'
                    }
                }
            }
        }
        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}