// This Jenkinsfile's main purpose is to show a real-world-ish example
// of what Pipeline config syntax actually looks like. 
pipeline {
    // Make sure that the tools we need are installed and on the path.
    agent{
         dockerfile {
            filename "POCDockerfile"
        //docker {
        
            label 'DEV'
            args "-v /tmp:/tmp -v /var/run/docker.sock:/var/run/docker.sock"
        }
    }

    tools {
    // Symbol for tool type and then name of configured tool installation
        maven "MAVEN3.3.9"
        jdk "JDK8u101"
    }

    post {
            always {
              echo 'This will always run outside'
              sh "sudo docker ps"
              sh "sudo docker images"
            }
            failure {
              echo 'This will run only if failed outside'
            }
            changed {
              echo 'This will run only if the state of the Pipeline has changed outside'
            }
    }

    stages {
        stage('checkout'){
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/yixiaol-m/POC-jenkins.git']]])
            }
        }
        stage('echo'){
            steps {
                echo 'hello from beta'
            }
        }
        // While there is only one stage here, you can specify as many stages as you like!
        stage("build") {
            steps {
                sh "sudo docker ps"
                sh 'id'
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
             sh 'fail me please'
         }
        }
    }
}

