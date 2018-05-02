pipeline {
    agent none
    environment {
        MAJOR_VERSION = 1
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '2', artifactNumToKeepStr: '1'))
    }

    stages {
        stage('git information') {
            agent any
            steps {
                echo "My Branch name: ${env.BRANCH_NAME}"
                echo "My Commit Number: ${env.GIT_COMMIT}"
            }
        }
        stage('build') {
            agent any
            steps {
                sh 'ant -f build.xml -v'
            }
        }
        stage('test') {
            agent any
            steps {
                sh 'make check || true'
                sh 'ant -f test.xml -v'
                junit 'reports/result.xml'
            }
        }

    }


    post {
        always {
            archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
        }
    }
}