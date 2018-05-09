properties([
    buildDiscarder(logRotator(numToKeepStr: '2')),
    disableConcurrentBuilds(),
    parameters([
        string(defaultValue: 'https://registry.mikaellindemann.dk', description: 'The registry URL', name: 'REGISTRY_URL'),
        string(defaultValue: 'Awia-Registry', description: 'The Jenkins ID of the credentials for the registry specified in REGISTRY_URL.', name: 'REGISTRY_CREDENTIALS_ID')
    ])
])

node('docker&&linux') {
    def cudaBase
    def cudaDevel
    def cudaAsp

    stage('Fetch') {
        checkout scm

        docker.image('ubuntu:18.04').pull()
        docker.image('microsoft/aspnetcore:2.0').pull()
    }
    stage('Base') {
        cudaBase = docker.build('cuda:9.1-base-ubuntu18.04', '-f dockerfiles/Dockerfile.base .')
    }
    stage('Devel') {
        cudaDevel = docker.build('cuda:9.1-devel-ubuntu18.04', '-f dockerfiles/Dockerfile.devel .')
    }
    stage('AspNetCore') {
        cudaAsp = docker.build('aspnetcore:2.0-cuda', '-f dockerfiles/Dockerfile.aspnetcore .')
    }
    stage('Push') {
        docker.withRegistry(params.REGISTRY_URL, params.REGISTRY_CREDENTIALS_ID) {
            if (env.BRANCH_NAME == 'master') {
                cudaBase.push()
                cudaDevel.push()
                cudaAsp.push()
            } else {
                cudaBase.push("cuda:9.1-base-ubuntu18.04-${env.BRANCH_NAME}")
                cudaDevel.push("cuda:9.1-devel-ubuntu18.04-${env.BRANCH_NAME}")
                cudaAsp.push("aspnetcore:2.0-cuda-${env.BRANCH_NAME}")
            }
        }
    }
}