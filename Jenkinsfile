node('agent1') {

    stage('Checkout') {
        checkout scm
    }

    stage('Build') {
        sh 'chmod +x gradlew || true'
        sh './gradlew build'
    }

    stage('Run') {
        sh 'java -cp build/classes/java/main App'
    }
}
