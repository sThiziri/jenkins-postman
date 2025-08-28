pipeline {
    agent any
    environment {
        BASE_URL = "http://192.168.1.95:8001"
        REPORTS_DIR = "reports"
    }
    stages {
        stage('Install Newman') {
            steps {
                sh 'npm install -g newman'
            }
        }
        stage('Prepare Reports Directory') {
            steps { sh 'mkdir -p ${REPORTS_DIR}' }
        }
        stage('Run home_test') {
            steps { 
                sh 'newman run home_test.postman_collection.json --env-var url=${BASE_URL} --reporters cli,html --reporter-html-export ${REPORTS_DIR}/home_test_report.html'
            }
        }
        stage('Run welcome_test') {
            steps { 
                sh 'newman run welcome_test.postman_collection.json --env-var url=${BASE_URL} --reporters cli,html --reporter-html-export ${REPORTS_DIR}/welcome_test_report.html'
            }
        }
        stage('Archive Reports') {
            steps { archiveArtifacts artifacts: "${REPORTS_DIR}/*.html" }
        }
    }
}
