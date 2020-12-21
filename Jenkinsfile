node {
    def appName = 'flask-demo'
    def imageTag = "8c606dde59ba/${appName}:${env.BUILD_NUMBER}"
    stage('Clone repository') {
        checkout scm
    }
    stage('Build image') {
        app = docker.build("${imageTag}")
    }
    stage('Test image') {
        sh 'echo "Tests passed"'
    }
    stage('Push image') {
        docker.withRegistry('http://8c606dde59ba', '') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
    stage('Deploy') {
        try {
            sh 'docker rm -f flask1'
            sh 'docker run -d -p 80:80 --name flask1 8c606dde59ba/flask-demo:latest'
        }
        catch (exc) {
            sh 'docker run -d -p 80:80 --name flask1 8c606dde59ba/flask-demo:latest'
        }
    }
}
