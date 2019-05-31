node {
    def dockerImage

    stage('Cloning') {

        /* Clone the repository to the workspace */
        checkout scm
    }

    stage('BuildingImage') {
        /* This builds the actual image.
         * Synonymous to docker build on the command line:
         * docker build -t nethali/hellonode .
         * Default tag latest will be used. */
        dockerImage = docker.build("nethali/hellonode")
    }

    stage('TestingImage') {
        /* Ideally, The test framework should run against the image.
         * Just simulate it here */
        sh 'echo Testing of image'
    }

    stage('PushingImage') {
        /* Finally, Image is pushed with two tags
         * First, the incremental build number from Jenkins. This create new Tag for image
         * Second, the 'latest' tag. This was already created
         * Pushing multiple tags is cheap, as all the layers are reused */
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            dockerImage.push("${env.BUILD_NUMBER}")
            dockerImage.push("latest")
        }
    }
}
