pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image') {
            when {
                branch 'master'
            }
            steps {
                scripts {
                    app = docker.build('therecomed1940/train-schedule')
                    app.inside {
                        sh 'echo $(curl http://localhost:8080)'
                    }
                }
            }
        }
        stage('Push Docker Image to Registry') {
            when {
                branch 'master'
            }
            steps {
                scripts {
                    docker.withRegistry('https://docker.mycorp.com/', 'docker_hub_login') {
                        docker.push("${env.BUILD_NUMBER}")
                        docker.push('latest')
                    }
                }
            }
        }
    }
}
