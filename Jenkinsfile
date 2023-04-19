pipeline {
    agent any
    stages {
        stage('Clone Repo from github') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/mzm2018/hello-world.git']])
            }
        }
        stage('Coverity Scan') {
            steps {
                withCoverityEnvironment(coverityInstanceUrl: 'https://mzm-XPS-13-9380:8443', createMissingProjectsAndStreams: true, projectName: 'develop_mzm_project2', streamName: 'develop_mzm_project2', viewName: 'High Impact Outstanding') {
    // some block
}
            }
        }
        }
    }
