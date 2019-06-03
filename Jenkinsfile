node {
    def dockerImage

    stage('Cloning') {
        /* Clone the repository to the workspace */
        checkout scm
    }

    stage('BuildingImage') {
        /* This builds the actual image.
         * Synonymous to docker build on the command line:
         * docker build -t nethali/simplewebapp .
         * Default tag latest will be used.
         * New image created: nethali/simplewebapp:latest */
        dockerImage = docker.build("nethali/simplewebapp", "--network host")
    }

    stage('TestingImage') {
        /* Ideally, The test framework should run against the image.
         * Just simulate it here */
        sh 'echo Testing of image'
    }

    stage('PushingImage') {
        /* Finally, Image is pushed with two tags
         * First, the incremental build number from Jenkins.
         * Second, the 'latest' tag.
         * Two new images with Docker Registry URL will be created:
         * registry.hub.docker.com/nethali/simplewebapp:${env.BUILD_NUMBER}
         * registry.hub.docker.com/nethali/simplewebapp:latest
         * Pushing multiple tags is cheap, as all the layers are reused
         * docker-hub-credentials should be defined in Jenkins for Docker Account */
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            dockerImage.push("${env.BUILD_NUMBER}")
            dockerImage.push("latest")
        }
    }
}
