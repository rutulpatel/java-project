pipeline {
    agent none
    environment {
        MAJOR_VERSION = 1
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '2', artifactNumToKeepStr: '1'))
    }

    stages {
        stage('Git Info') {
            agent any
            steps {
                echo "My Branch name: ${env.BRANCH_NAME}"
                echo "My Commit Number: ${env.GIT_COMMIT}"
            }
        }
        stage('Unit Tests') {
            agent {
                label 'CentOS'
            }
            steps {
                sh 'make check || true'
                sh 'ant -f test.xml -v'
                junit 'reports/result.xml'
            }
        }
        stage('Build') {
            agent {
                label 'Apache'
            }
            steps {
                sh 'ant -f build.xml -v'
            }
            post {
                success {
                    archiveArtifacts artifacts: 'dist/rectangle*.jar', fingerprint: true
                }
            }
        }
        stage('Deploy') {
            agent {
                label 'Apache'
            }
            steps {
                sh "if ![ -d '/var/www/html/rectangles/all/${env.BRANCH_NAME}' ]; then mkdir /var/www/html/rectangles/all/${env.BRANCH_NAME}; fi"
                sh "cp dist/rectangle_${env.MAJOR_VERSION}.${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/${env.BRANCH_NAME}/"
            }
        }


    }

    post {
        always {
            echo "end of file"
        }
    }
}