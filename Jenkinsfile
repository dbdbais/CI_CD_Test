pipeline {
    agent any

    environment {
        JAR_FILE = 'target/*.jar'
        PROJECT_DIR = 'kidswallet'
        IMAGE_NAME = 'rain5191/kidswallet'
    }

    stages {
        stage('Checkout') {
            steps {
                // GitLab에서 소스 코드 체크아웃 (자격 증명 추가)
                git credentialsId: 'ci_cd_test', branch: 'master', url: 'https://lab.ssafy.com/rain5191/cicd_test.git'
            }
        }
        
      stage('Build') {
            steps {
               dir("${PROJECT_DIR}") {
                    sh 'chmod +x ./gradlew' // Gradle Wrapper 실행 권한 부여
                    sh './gradlew clean build' // 빌드 실행
                        echo 'Build completed successfully!'
                }
            }
        }
         stage('Docker Build') {
            steps {
                script {
                     sh "docker build -t ${IMAGE_NAME} ${PROJECT_DIR}" // Dockerfile이 있는 디렉토리 지정
                }
            }
        }
        stage('Run Container') {
             steps {
        script {
            sh 'docker stop kidswallet-container || true'
            sh 'docker rm kidswallet-container || true'
            sh "docker run -d -p 8000:8000 --name kidswallet-container ${IMAGE_NAME}:latest"
        }
    }
        }
    }
    post {
        success {
          echo 'Build, Docker image creation, and deployment were successful!'
        }
        failure {
       echo 'Build or Docker image creation failed!'
        }
    }
}
