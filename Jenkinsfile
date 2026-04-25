properties([
    parameters([
        string(name: 'BRANCH_NAME', defaultValue: 'develop', description: 'Git branch to build'),
        string(name: 'IMAGE_NAME', defaultValue: 'gradle-app', description: 'Docker image name')
    ])
])

node {

    def containerName = "gradle-${params.BRANCH_NAME}"
    def imageTag = "${params.IMAGE_NAME}:${BUILD_NUMBER}"

    stage('Checkout') {
        git branch: "${params.BRANCH_NAME}",
        url: 'https://github.com/shanchalsenthil/gradle-jenkins-demo.git'
    }

    stage('Build') {
        sh 'chmod +x gradlew || true'
        sh './gradlew clean build'
    }

    stage('Build Docker Image') {
        sh "docker build -t ${imageTag} ."
    }

    stage('Stop Old Container') {
        sh """
        docker stop ${containerName} || true
        docker rm ${containerName} || true
        """
    }

    stage('Run Container') {
        sh """
        docker run -d \
        --name ${containerName} \
        -p 0:8080 \
        ${imageTag}
        """
    }
}
