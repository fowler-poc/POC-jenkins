// This Jenkinsfile's main purpose is to show a real-world-ish example
// of what Pipeline config syntax actually looks like. 
pipeline {
    // Make sure that the tools we need are installed and on the path.
    agent{
        dockerfile {
            filename "POCDockerfile"
            label 'OA-JNLP'
            args "--group-add 3001"
        }      
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
                sh 'whoami'
                sh 'id'
                sh 'cat /etc/group'
		sh 'df -h'
		sh 'java -version'
		sh 'which java'
		sh 'mvn -version'
		sh 'which mvn'
		sh 'docker ps'
		sh 'docker run -d -t docker.io/ubuntu:16.04 /bin/cat'
            }
        }
        // While there is only one stage here, you can specify as many stages as you like!
        stage("build") {
            agent{
                dockerfile {
                    filename "POCDockerfile"
                    label 'OA-JNLP'
                    args "--group-add 3001"
                }      
            }
            steps {
	        echo 'Hello from inside container'
                sh "whoami"
                sh 'id'
                sh 'cat /etc/group'
                sh 'df -h'
		sh 'java -version'
		sh 'which java'
		sh 'mvn -version'
		sh 'which mvn'
		sh 'docker ps'
		sh 'docker run -d -t docker.io/ubuntu:16.04 /bin/cat'
            }
        }
        stage('test') {
            post {
                always {
                  echo 'This will always run'
                  sh "docker ps"
                  sh "docker images"
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
