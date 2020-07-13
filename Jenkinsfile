pipeline {
    agent none
    triggers {
      cron '0 0 * * *'
    }
    options {
      disableConcurrentBuilds()
    }
    stages{
        stage("Tyko"){
            steps{
                build(
                    job: 'OpenSourceProjects/tyko/master',
                      parameters: [
                        booleanParam(name: 'FRESH_WORKSPACE', value: false),
                        booleanParam(name: 'BUILD_CLIENT', value: true),
                        booleanParam(name: 'TEST_RUN_TOX', value: true),
                        booleanParam(name: 'DEPLOY_SERVER', value: false)
                      ]
                    )
            }
        }
        stage("HathiZip"){
            steps{
                build(
                    job: 'OpenSourceProjects/HathiZip/master',
                    parameters: [
                        string(name: 'PROJECT_NAME', value: 'HathiTrust Zip for Submit'),
                        booleanParam(name: 'TEST_RUN_TOX', value: true),
                        booleanParam(name: 'PACKAGE_CX_FREEZE', value: true),
                        booleanParam(name: 'DEPLOY_DEVPI', value: true),
                        booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                        booleanParam(name: 'DEPLOY_ADD_TAG', value: false),
                        booleanParam(name: 'DEPLOY_SCCM', value: false),
                        booleanParam(name: 'UPDATE_DOCS', value: false)
                    ]
                )
            }
        }
        stage("pyhathiprep"){
            steps{
                build(
                    job: 'OpenSourceProjects/pyhathiprep/master',
                    parameters: [
                        booleanParam(name: 'TEST_RUN_TOX', value: true),
                        booleanParam(name: 'DEPLOY_DEVPI', value: true),
                        booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                        string(name: 'URL_SUBFOLDER', value: 'pyhathiprep'),
                        booleanParam(name: 'DEPLOY_DOCS', value: false),
                        booleanParam(name: 'DEPLOY_ADD_TAG', value: false)
                    ]
                )
            }
        }
        stage("HT_checksum_update"){
            steps{
                build(
                    job: 'OpenSourceProjects/HT_checksum_update/master',
                    parameters: [
                        string(name: 'PROJECT_NAME', value: 'HathiTrust Checksum Updater'),
                        booleanParam(name: 'PACKAGE_CX_FREEZE', value: true),
                        booleanParam(name: 'DEPLOY_SCCM', value: false),
                        booleanParam(name: 'DEPLOY_DEVPI', value: true),
                        booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                        booleanParam(name: 'UPDATE_DOCS', value: false),
                        string(name: 'URL_SUBFOLDER', value: 'hathi_checksum_updater')
                    ]
                )
            }
        }
        stage("uiucprescon.images"){
            steps{
                build(
                    job: 'OpenSourceProjects/imageprocess/master',
                    parameters: [
                        booleanParam(name: 'TEST_RUN_TOX', value: true),
                        booleanParam(name: 'DEPLOY_DEVPI', value: true),
                        booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                        booleanParam(name: 'DEPLOY_ADD_TAG', value: false),
                        booleanParam(name: 'DEPLOY_DOCS', value: false)
                    ]
                )
            }
        }
    }


}
