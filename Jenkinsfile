MY_DOCKER_VERSION = 'MY_NULL'
MY_PATH = 'MY_NULL'
MY_MACHINE_NAME = 'MY_NULL'
IS_SUCCESS = 'MY_NULL'
SECOND_SCRIPT_RESULT = 'MY_NULL'

pipeline {
  agent any
  stages {
    stage('one') {
        steps {
            echo '1'
            sh 'docker --version' 
            script {
                MY_DOCKER_VERSION = sh(script: 'docker --version', returnStdout: true).trim()
                MY_PATH = sh(script: 'pwd', returnStdout: true).trim()
            }
            echo "MY_DOCKER_VERSION is ${MY_DOCKER_VERSION}"
            echo "MY_PATH is ${MY_PATH}"
        }
    }
    stage('two') {
        steps {
            echo '2'
            echo "2-MY_DOCKER_VERSION is ${MY_DOCKER_VERSION}"
            echo "2-MY_PATH is ${MY_PATH}"
            script {
                sh 'chmod +x bScript.sh'
                MY_MACHINE_NAME = sh(script: './bScript.sh', returnStdout: true).trim()
                IS_SUCCESS = sh(script: './bScript.sh', returnStatus: true) == 0
            }
            echo "2-MY_MACHINE_NAME is ${MY_MACHINE_NAME}"
            echo "2-IS_SUCCESS is ${IS_SUCCESS}"
        }

    }
    stage('three') {
        when {
        expression { IS_SUCCESS != false }
        }
        steps {
            echo '3'
            echo "3-MY_DOCKER_VERSION is ${MY_DOCKER_VERSION}"
            echo "3-MY_PATH is ${MY_PATH}"
            script {
                sh 'chmod +x secondScript.sh'
                SECOND_SCRIPT_RESULT = sh(script: "./secondScript.sh ${MY_MACHINE_NAME}", returnStdout: true).trim()
            }
            echo "3-SECOND_SCRIPT_RESULT is ${SECOND_SCRIPT_RESULT}"
        }
    }
    stage('four') {
        when {
            expression { IS_SUCCESS !=true }
        }
        steps {
            echo '4'
            echo "4-MY_DOCKER_VERSION is ${MY_DOCKER_VERSION}"
            echo "4-MY_PATH is ${MY_PATH}"
            echo "test"
        }
    }
    stage('five') {
        when{
            anyOf{
                allOf{
                    expression { IS_SUCCESS != true }
                    expression { 0 == 1 }
                }
            expression{ SECOND_SCRIPT_RESULT.contains("man")}
            }
    }
        steps {
            echo '5 ve tese'
            echo "5-MY_DOCKER_VERSION is ${MY_DOCKER_VERSION}"
            echo "5-MY_PATH is ${MY_PATH}"
        }
    }
  }
}
