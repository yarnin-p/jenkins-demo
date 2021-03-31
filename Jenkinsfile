#!/bin/bash +x

def getEnvName() {
    def branch = scm.branches[0].name

    switch(branch) {
        case "*/main":
            result = "prod"
            break
        case "*/sit":
            result = "sit"
            break
        case "*/staging":
            result = "staging"
            break
        case "*/develop":
            result = "develop"
            break
    }
    return result
}

def envName = getEnvName()

def actionDeploy(env) {
    switch(env) {
        case "prod":
            sh '. ./build-prod.sh'
            break
        case "staging":
            break
        case "sit":
            sh '. ./build-sit.sh'
            break
        case "develop":
            sh '. ./build-dev.sh'
            break
    }
}

pipeline {
    agent none
    stages {
        stage('Initial'){
            agent any
            steps {
                echo 'Initial........'
                echo '------------------------------------------------------------------'
            }
        }
        stage('Deployment') {
            agent any
            steps {
                echo "Deploying.....${envName}"
                actionDeploy(envName)
                echo "------------------------------------------------------------------"
            }
        }
    }


    post {
        success {
            echo "SUCCESSFULLY"
        }

        failure {
            echo "FAILED"
        }
    }
}
