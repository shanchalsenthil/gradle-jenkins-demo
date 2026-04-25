properties([
    parameters([
        string(name: 'BRANCH_NAME', defaultValue: 'develop', description: 'Git branch to build'),
        string(name: 'IMAGE_NAME', defaultValue: 'gradle-app', description: 'Docker image name')
    ])
])

node {

    stage('Checkout') {
        git branch: "${params.BRANCH_NAME}", url: 'https://github.com/shanchalsenthil/gradle-jenkins-demo.git'
    }

    stage('Build') {
        sh 'chmod +x gradlew || true'
        sh './gradlew clean build'
    }

    stage('Build Docker Image') {
        sh "docker build -t ${params.IMAGE_NAME} ."
    }

    stage('Run Container') {
        sh """
        docker stop gradle-container || true
        docker rm gradle-container || true
        docker run -d -p 8085:8080 --name gradle-container ${params.IMAGE_NAME}
        """
    }
}
