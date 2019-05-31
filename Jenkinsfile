node {
    def app

    stage('Cloning') {

        /* Clone the repository to the workspace */
        checkout scm
    }

    stage('BuildingImage') {
        /* This builds the actual image.
         * Synonymous to docker build on the command line:
         * docker image build -t nethali/hellonode:${BUILD_NUMBER} . */
        app = docker.build("nethali/hellonode")
    }

    stage('TestingImage') {
        /* Ideally, The test framework should run against the image.
         * Just simulate it here */
        app.inside {
            sh 'echo "Testing"'
        }
    }

    stage('PushingImage') {
        /* Finally, Image is pushed with two tags
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag
         * Pushing multiple tags is cheap, as all the layers are reused */
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}
