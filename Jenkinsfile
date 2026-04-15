node {

    stage('Checkout') {
        checkout scm
    }

    stage('Build') {
        sh 'chmod +x gradlew || true'
        sh './gradlew clean build'
    }

    stage('Run') {
        sh '''
        pkill -f gradle-jenkins-demo || true
        nohup java -jar build/libs/*.jar > app.log 2>&1 &
        '''
    }
}
