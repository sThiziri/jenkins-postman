pipeline {
    agent {
        docker {
            image 'postman/newman:alpine' // Image officielle Newman
        }
    }

    environment {
        BASE_URL = "http://192.168.1.95:8001" // Modifie selon ton API
    }

    stages {
        stage('Run Postman Collection - home_test') {
            steps {
                sh """
                newman run home_test.json \\
                    --env-var "url=${BASE_URL}" \\
                    --reporters cli,html \\
                    --reporter-html-export home_test_report.html
                """
            }
        }

        stage('Run Postman Collection - welcome_test') {
            steps {
                sh """
                newman run welcome_test.json \\
                    --env-var "url=${BASE_URL}" \\
                    --reporters cli,html \\
                    --reporter-html-export welcome_test_report.html
                """
            }
        }

        stage('Archive Reports') {
            steps {
                archiveArtifacts artifacts: '*.html', fingerprint: true
            }
        }
    }

    post {
        always { echo "Tests finished !" }
        success { echo "All tests passed" }
        failure { echo "Some tests failed" }
    }
}
