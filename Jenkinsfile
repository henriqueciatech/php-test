node {

    /**
     * Constants
     */

    final DOCKER_CREDENTIALS = "dockerhub"
    final DOCKER_REGISTRY    = "https://index.docker.io/v1/"

    /**
     * Tags and branches
     */

    final ENVIRONMENT_DEVELOPMENT = /(development)/

    /**
     * Settings
     */

    final PROJECT_NAME = "php-test"

    /**
     * Variables
     */

    final IMAGE_NAME = "ciatechrancher/${PROJECT_NAME}"
    final IMAGE_TAG = env.BRANCH_NAME.replace("/", "-")
    final IMAGE_REF = "${IMAGE_NAME}:${IMAGE_TAG}"

    /**
     * Deployment keys
     */

    final DEPLOY_KEY_DEVELOPMENT = "7bfd636c-bc36-431b-95a8-7cafa4366835"

    /**
     * Deployment urls
     */

    final DEPLOY_URL_DEVELOPMENT = "https://deploy-development.rancher.ciatech.com.br/deploy/${DEPLOY_KEY_DEVELOPMENT}?image=${IMAGE_REF}"

    /**
     * Steps
     */

    def container

    /**
     * Source code checkout
     */

    stage("Checkout") {
        checkout scm
    }

    /**
     * Builds the project image
     **/

    stage("Build image") {
        container = docker.build(IMAGE_NAME)
    }

    /**
     * Pushes the image to the registry
     **/

    stage("Push image") {

        def shouldPush = false

        if (
            env.BRANCH_NAME ==~ ENVIRONMENT_DEVELOPMENT
        ) {
            shouldPush = true
        }

        if (shouldPush) {
            docker.withRegistry(DOCKER_REGISTRY, DOCKER_CREDENTIALS) {
                container.push(IMAGE_TAG)
            }
        }

    }

    /**
     * Deploys the image to the server.
     **/

    stage("Deploy image") {

        if (env.BRANCH_NAME ==~ ENVIRONMENT_DEVELOPMENT) {

            sh "curl --fail ${DEPLOY_URL_DEVELOPMENT}"

        } else if (env.BRANCH_NAME ==~ ENVIRONMENT_STAGING) {

            sh "curl --fail ${DEPLOY_URL_STAGING}"

        } else if (env.BRANCH_NAME ==~ ENVIRONMENT_PRODUCTION) {

            sh "curl --fail ${DEPLOY_URL_PRODUCTION}"

        }

    }

}