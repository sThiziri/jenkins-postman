pipeline {
    agent {
        docker {
            image 'postman/newman:latest'
            args '--entrypoint='
        }
    }

    environment {
        BASE_URL = "http://192.168.1.95:8001"
        REPORTS_DIR = "reports"
    }

    stages {
        stage('Prepare Reports Directory') {
            steps {
                sh(script: "mkdir -p ${REPORTS_DIR}", shell: "/bin/sh")
            }
        }

        stage('Run Postman Collection - home_test') {
            steps {
                sh(script: """
                    newman run home_test.postman_collection.json \
                        --env-var 'url=${BASE_URL}' \
                        --reporters cli,html \
                        --reporter-html-export ${REPORTS_DIR}/home_test_report.html
                """, shell: "/bin/sh")
            }
        }

        stage('Run Postman Collection - welcome_test') {
            steps {
                sh(script: """
                    newman run welcome_test.postman_collection.json \
                        --env-var 'url=${BASE_URL}' \
                        --reporters cli,html \
                        --reporter-html-export ${REPORTS_DIR}/welcome_test_report.html
                """, shell: "/bin/sh")
            }
        }

        stage('Archive Reports') {
            steps {
                archiveArtifacts artifacts: "${REPORTS_DIR}/*.html", fingerprint: true
            }
        }
    }

    post {
        always { echo "Tests finished !" }
        success { echo "All tests passed" }
        failure { echo "Some tests failed" }
    }
}
