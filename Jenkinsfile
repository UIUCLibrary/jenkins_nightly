pipeline {
    agent none
    stages{
        stage("Tyko"){
            steps{
                build job: 'OpenSourceProjects/tyko/master', parameters: [booleanParam(name: 'FRESH_WORKSPACE', value: false), booleanParam(name: 'BUILD_CLIENT', value: false), booleanParam(name: 'TEST_RUN_TOX', value: true), booleanParam(name: 'DEPLOY_SERVER', value: false)]
            }
        }
    }
}
