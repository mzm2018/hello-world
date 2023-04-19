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
                withCoverityEnvironment(coverityInstanceUrl: 'https://mzm-XPS-13-9380:8443', createMissingProjectsAndStreams: true, projectName: 'develop_mzm_project2', streamName: 'develop_mzm_project2', viewName: 'Outstanding Issues') {
    sh "echo $COV_STREAM"
    sh "cov-build --dir data /home/mzm/Devops/apache-maven-3.6.3/bin/mvn clean install"
    sh "cov-analyze --dir date --aggressiveness-level high --all --distrust-all
    sh "cov-commit-defects --dir data --url https://mzm-XPS-13-9380:8443/
    coverityIssueCheck coverityInstanceUrl: 'https://mzm-XPS-13-9380:8443', markUnstable: true, projectName: 'develop_mzm_project1', viewName: 'Outstanding Issues'
}
            }
        }
        }
    }