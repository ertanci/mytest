pipeline {
  agent any
  stages {
    stage('one') {
        steps {
            echo '1'
            sh 'docker --version' 
            script {
                REMOTE_ARRAY = sh(script: 'sshpass -p 'Ertan123' ssh ertan@10.0.2.4 uname', returnStdout: true).trim()
                MY_DOCKER_VERSION = sh(script: 'docker --version', returnStdout: true).trim()
                MY_PATH = sh(script: 'pwd', returnStdout: true).trim()
            }
            echo "REMOTE_ARRAY is ${REMOTE_ARRAY}"
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
            build job: "mytest2/main", propagate: true, wait: true
            build job: 'job1', parameters: [[$class: 'StringParameterValue', name: 'HOSTNAME', value: "${SECOND_SCRIPT_RESULT}"],[$class: 'StringParameterValue', name: 'CHECK', value: "ERTAN"],]
        }
    }
  }

  environment {
    MY_DOCKER_VERSION = 'MY_NULL'
    MY_PATH = 'MY_NULL'
    MY_MACHINE_NAME = 'MY_NULL'
    IS_SUCCESS = 'MY_NULL'
    SECOND_SCRIPT_RESULT = 'MY_NULL'
    REMOTE_ARRAY= 'MY_NULL'
  }
}
