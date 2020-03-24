node {
    stage('Checkout') {
        checkout scm
    }

    stage('Build Image'){
        def customImage = docker.build("becram/build-container:latest")
    }

    stage('Push Image') {
        def JENKIN_VERSION = sh returnStdout: true, script: "cat Dockerfile | head -n 1 | awk -F ':' '{print \$2}' | awk -F '-' '{print \$1}'"
        withCredentials([usernamePassword(
            credentialsId: "dockerhub-becram",
            usernameVariable: "USER",
            passwordVariable: "PASS"
        )]) {
            sh "docker login -u $USER -p $PASS"
        }

        sh "docker tag becram/build-container:latest becram/build-container:$BUILD_NUMBER"
        sh "docker tag becram/build-container:latest becram/build-container:$JENKINS_VERSION"
        sh "docker push becram/build-container:latest"
        sh "docker push becram/build-container:$BUILD_NUMBER"
    }
}
