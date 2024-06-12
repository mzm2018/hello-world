pipeline {
    agent any
    
    stages {
       stage('CleanWorkspace') {
            steps {
                cleanWs()
            }
        }
        stage('Clone Repo from github') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/mzm2018/hello-world.git']])
               //   checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/mzm2018/WebGoat.git']])         
	    }                                       
        }
        stage('Coverity Scan') {
            steps {
                withCoverityEnvironment(coverityInstanceUrl: 'http://mzm-XPS-13-9380:8080', createMissingProjectsAndStreams: true, projectName: 'develop_mzm_project1', streamName: 'develop_mzm_project1_stream', viewName: 'Outstanding Issues') {
    sh "echo $COV_STREAM"
    //sh "cov-build --dir data /home/mzm/Devops/apache-maven-3.6.3/bin/mvn clean install -DskipTests=true"
    sh "cov-capture --project-dir /var/lib/jenkins/workspace/coverity_job --dir /var/lib/jenkins/workspace/coverity_job/idir"
    //sh "cov-analyze --dir data --all --webapp-security"
    sh "cov-analyze --dir /var/lib/jenkins/workspace/coverity_job/idir"
    //sh "cov-commit-defects --dir data --url http://mzm-XPS-13-9380:8080/ --stream develop_mzm_project4_stream --user admin --password Coverity@2023"
    sh "cov-commit-defects --dir /var/lib/jenkins/workspace/coverity_job/idir --stream develop_mzm_project1_stream --user admin --password Coverity@2023 --url http://mzm-XPS-13-9380:8080/"                
    coverityIssueCheck coverityInstanceUrl: 'http://mzm-XPS-13-9380:8080', markUnstable: true, projectName: 'develop_mzm_project1', viewName: 'Outstanding Issues'
}
            }
        }
     stage ('Run Seeker IAST') {
	     steps {
                // seeker install.sh is located in the parent directory of current WebGoat folder
                // the install.sh script was downloaded from Seeker UI from Connect Agent Wizard
                sh "../install.sh"
                sh "java '-javaagent:./seeker/seeker-agent.jar' -jar ./webapp/target/webapp.war &"
                sh "ls -al webapp/target/"
                
                timeout(time:1, unit:'DAYS') {
				    input message:'Finished Seeker Testing ?'
			    }
            }
     }  
        }
    }
