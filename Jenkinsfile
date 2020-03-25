node {
    stage('Checkout') {
        checkout scm
    }

    stage('Build Image'){
        def customImage = docker.build("becram/build-container:latest")
    }

    stage('Push Image') {
        withCredentials([usernamePassword(
            credentialsId: "dockerhub-becram",
            usernameVariable: "USER",
            passwordVariable: "PASS"
        )]) {
            sh "docker login -u $USER -p $PASS"
        }

        sh "docker tag becram/build-container:latest becram/build-container:$BUILD_NUMBER"
        sh "docker tag becram/build-container:latest becram/build-container:$GIT_COMMIT"
        sh "docker push becram/build-container:latest"
        sh "docker push becram/build-container:$BUILD_NUMBER"
        sh "docker push becram/build-container:$GIT_COMMIT"
    }
}
