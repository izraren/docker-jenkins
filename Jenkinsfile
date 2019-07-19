ode {
    def appName = 'flask-demo'
    def imageTag = "localhost:5000/${appName}:${env.BUILD_NUMBER}"
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
        docker.withRegistry('http://localhost:5000', '') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
    stage('Deploy') {
        try {
            sh 'docker rm -f flask1'
            sh 'docker run -d -p 80:80 --name flask1 localhost:5000/flask-demo:latest'
        }
        catch (exc) {
            sh 'docker run -d -p 80:80 --name flask1 localhost:5000/flask-demo:latest'
        }
    }
}
