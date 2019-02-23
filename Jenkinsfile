podTemplate(label: 'docker-omnicore', containers: [
  containerTemplate(name: 'docker', image: 'docker', ttyEnabled: true, command: 'cat', envVars: [
    envVar(key: 'DOCKER_HOST', value: 'tcp://docker-host-docker-host:2375')
  ])
]) {
  node('docker-omnicore') {
    stage('Build Image') {
      container('docker') {
        def scmVars = checkout scm
        def VERSION = "0.4.0-alpine"
        dir("${VERSION}") {
          sh "docker build -t santiment/omnicore:${VERSION} ."

          if (env.BRANCH_NAME == "master") {
            withDockerRegistry([ credentialsId: "dockerHubCreds", url: "" ]) {
              sh "docker push santiment/omnicore:${VERSION}"
            }
          }
        }
      }
    }
  }
}
