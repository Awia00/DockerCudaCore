properties([
    buildDiscarder(logRotator(numToKeepStr: '2')),
    disableConcurrentBuilds(),
    parameters([
        string(defaultValue: 'https://registry.mikaellindemann.dk', description: 'The registry URL', name: 'REGISTRY_URL'),
        string(defaultValue: 'Awia-Registry', description: 'The Jenkins ID of the credentials for the registry specified in REGISTRY_URL.', name: 'REGISTRY_CREDENTIALS_ID')
    ])
])

node('docker&&linux') {
    def cudaAsp

    stage('Fetch') {
        checkout scm

        docker.image('nvidia/cuda:9.2-ubuntu18.04').pull()
    }
    stage('AspNetCore') {
        cudaAsp = docker.build('aspnetcore:2.0-cuda-9.2', '-f dockerfiles/Dockerfile.aspnetcore .')
    }
    stage('Push') {
        docker.withRegistry(params.REGISTRY_URL, params.REGISTRY_CREDENTIALS_ID) {
            if (env.BRANCH_NAME == 'master') {
                cudaAsp.push()
            } else {
                cudaAsp.push("aspnetcore:2.0-cuda-9.2-${env.BRANCH_NAME}")
            }
        }
    }
}
