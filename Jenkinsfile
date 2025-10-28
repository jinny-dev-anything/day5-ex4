pipeline {
    agent any

    environment {
        // 필요한 경우 Python 환경 경로 설정
        PYTHONPATH = "${WORKSPACE}"
    }

    stages {
        stage('Checkout') {
            steps {
                // Git repository에서 코드 체크아웃
                checkout scm
            }
        }

        stage('Set Up Python Environment') {
            steps {
                // Python 의존성 설치
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Run pytest') {
            steps {
                // pytest 실행하여 테스트 수행, JUnit XML 리포트 생성
                sh '''
                    . venv/bin/activate
                    pytest tests/ --junitxml=pytest-report.xml
                '''
            }
        }
    }

    post {
        always {
            // JUnit 테스트 결과 보고서 Jenkins에 게시
            junit 'pytest-report.xml'

            // 테스트 실패 시 이메일 등 추가 알림 설정 가능 (선택적)
            // emailext(...) 등을 통해 이메일 알림을 설정할 수 있습니다.
        }

        success {
            echo '테스트 자동화가 성공적으로 완료되었습니다!'
        }

        failure {
            echo '테스트 자동화 중 일부 테스트가 실패했습니다. 리포트를 확인해주세요.'
        }
    }
}
