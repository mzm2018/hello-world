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
    sh "echo $COV_STREAM"
    sh "cov-build --dir data /home/mzm/Devops/apache-maven-3.6.3/bin/mvn clean install"
    sh "cov-analyze --dir date --aggressiveness-level high --all --distrust-all --enable-audit-mode --rule --security --webapp-security-aggressiveness-level high"
    sh "cov-commit-defects --dir data --url https://mzm-XPS-13-9380:8443/ --stream develop_mzm_project2 --user admin --password Coverity@2023"
    coverityIssueCheck coverityInstanceUrl: 'https://mzm-XPS-13-9380:8443', projectName: 'develop_mzm_project2', viewName: 'High Impact Outstanding'
}
            }
        }
        }
    }
