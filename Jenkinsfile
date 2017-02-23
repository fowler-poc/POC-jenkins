// This Jenkinsfile's main purpose is to show a real-world-ish example
// of what Pipeline config syntax actually looks like. 
pipeline {
    // Make sure that the tools we need are installed and on the path.
    agent{
	label 'OA'
    }

    tools {
    // Symbol for tool type and then name of configured tool installation
        maven "MAVEN3.3.9"
        jdk "JDK8u121"
    }

    stages {
        stage('checkout'){
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/HA']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/yixiaol-m/POC-jenkins.git']]])
            }
        }
        stage('echo'){
            steps {
                echo 'hello from beta'
                sh 'df -h'
		sh 'java -version'
		sh 'which java'
		sh 'mvn -version'
		sh 'which mvn'
            }
        }
        // While there is only one stage here, you can specify as many stages as you like!
        stage("build") {
            agent{
		    dockerfile {
                    filename "POCDockerfile"
                    label 'OA'
                    args "-v /var/run/docker.sock:/var/run/docker.sock"
                }      
            }
            steps {
                sh "whoami"
                sh 'id'
                sh 'df -h'
		sh 'java -version'
		sh 'which java'
		sh 'mvn -version'
		sh 'which mvn'
            }
        }
        stage('test') {
            post {
                always {
                  echo 'This will always run'
                  sh "sudo docker ps"
                  sh "sudo docker images"
                }
                failure {
                  echo 'This will run only if failed'
                }
                changed {
                  echo 'This will run only if the state of the Pipeline has changed'
                }
             }

             steps {
                 echo 'last step here'
             }
        }
    }
}
