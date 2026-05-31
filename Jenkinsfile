pipeline {
    agent any // 어떤 에이전트(실행 서버)에서든 실행 가능

    tools {
        maven 'maven 3.9.12' // Jenkins에 등록된 Maven 3.9.12을 사용
    }

    environment {
        // 배포에 필요한 변수 설정
        DOCKER_IMAGE = "demo-app" // 도커 이미지 이름
        CONTAINER_NAME = "springboot-container" // 도커 컨테이너 이름
        JAR_FILE_NAME = "app.jar" // 복사할 JAR 파일 이름
        PORT = "8081" // 컨테이너와 연결할 포트

        REMOTE_USER = "ec2-user" // 원격(spring) 서버 사용자
        REMOTE_HOST = "54.180.5.200" // 원격(spring) 서버 IP(Public IP)
        
        REMOTE_DIR = "/home/ec2-user/deploy" // 원격 서버에 파일 복사할 경로
        SSH_CREDENTIALS_ID = "803dd59e-b3c3-49e2-a51d-9ec2abc79ea9" // Jenkins SSH 자격 증명 ID
    }



    stages {
        stage('Git Checkout') {
            steps { // step : stage 안에서 실행할 실제 명령어            
             // Jenkins가 연결된 Git 저장소에서 최신 코드 체크아웃
                checkout scm
            }
        }

        stage('Maven Build') {
            // 테스트는 건너뛰고 Maven 빌드
            steps {
            sh 'mvn clean package -DskipTests'
            // sh 'echo Hello' : 리눅스 명령어 실행
            }
        }

        stage('Prepare Jar') {
            steps {
                // 빌드 결과물인 JAR 파일을 지정한 이름(app.jar)으로 복사
                sh 'cp target/demo-0.0.1-SNAPSHOT.jar ${JAR_FILE_NAME}'
            }
        }
    }
}