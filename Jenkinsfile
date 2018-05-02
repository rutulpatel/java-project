pipeline {
    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '2', artifactNumToKeepStr: '1'))
    }

    stages {
        stage('build') {
            steps {
                sh 'ant -f build.xml -v'
            }
        }
        stage('test') {
            steps {
                sh 'make check || true'
                junit 'test.xml'
            }
        }

    }


    post {
        always {
            archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
        }
    }
}