properties([
    parameters([
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Enter Git branch name to build'),
        string(name: 'IMAGE_NAME', defaultValue: 'gradle-app', description: 'Docker image name')
    ])
])

node {

    def branchName = params.BRANCH_NAME
    def containerName = "gradle-${branchName}".replaceAll('/', '-')
    def imageTag = "${params.IMAGE_NAME}:${branchName}-${BUILD_NUMBER}".replaceAll('/', '-')

    stage('Checkout Dynamic Branch') {
        echo "Checking out branch: ${branchName}"

        git branch: branchName,
            url: 'https://github.com/shanchalsenthil/gradle-jenkins-demo.git'
    }

    stage('Build Gradle Project') {
        sh 'chmod +x gradlew || true'
        sh './gradlew clean build'
    }

    stage('Build Docker Image') {
        echo "Building Docker image: ${imageTag}"
        sh "docker build -t ${imageTag} ."
    }

    stage('Stop Old Container') {
        echo "Stopping old container: ${containerName}"

        sh """
        docker stop ${containerName} || true
        docker rm ${containerName} || true
        """
    }

    stage('Run Docker Container') {
        echo "Running container from image: ${imageTag}"

        sh """
        docker run -d \
        --name ${containerName} \
        -p 0:8080 \
        ${imageTag}
        """
    }
}
