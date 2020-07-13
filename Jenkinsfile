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
        stage("uiucprescon.packager"){
            steps{
                build(
                    job: 'OpenSourceProjects/Packager/master',
                    parameters: [
                        booleanParam(name: 'TEST_RUN_TOX', value: true),
                        booleanParam(name: 'USE_SONARQUBE', value: true),
                        booleanParam(name: 'DEPLOY_DEVPI', value: true),
                        booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                        booleanParam(name: 'DEPLOY_ADD_TAG', value: false),
                        booleanParam(name: 'DEPLOY_DOCS', value: false),
                        string(name: 'DEPLOY_DOCS_URL_SUBFOLDER', value: 'packager')
                    ]
                )
            }
        }
        stage("uiucprescon.imagevalidate"){
            steps{
                build(
                    job: 'OpenSourceProjects/imagevalidate/master',
                    parameters: [
                        booleanParam(name: 'TEST_RUN_TOX', value: true),
                        booleanParam(name: 'DEPLOY_DEVPI', value: true),
                        booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                        booleanParam(name: 'DEPLOY_DOCS', value: false),
                        string(name: 'DEPLOY_DOCS_URL_SUBFOLDER', value: 'imagevalidate')
                    ]
                )
            }
        }
        stage("pyexiv2bind2"){
            steps{
                build(
                    job: 'OpenSourceProjects/pyexiv2bind2/master',
                    parameters: [
                        booleanParam(name: 'TEST_RUN_TOX', value: true),
                        booleanParam(name: 'USE_SONARQUBE', value: true),
                        booleanParam(name: 'DEPLOY_DEVPI', value: true),
                        booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                        string(name: 'URL_SUBFOLDER', value: 'py3exiv2bind'),
                        booleanParam(name: 'DEPLOY_DOCS', value: false)
                    ]
                )
            }
        }
        stage("DCCMedusaPackager"){
            steps{
                build(
                    job: 'OpenSourceProjects/DCCMedusaPackager/master',
                    parameters: [
                        string(name: 'PROJECT_NAME', value: 'Medusa Packager'),
                        booleanParam(name: 'PACKAGE_CX_FREEZE', value: true),
                        booleanParam(name: 'DEPLOY_DEVPI', value: true),
                        booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                        booleanParam(name: 'DEPLOY_SCCM', value: false),
                        booleanParam(name: 'UPDATE_DOCS', value: false),
                        string(name: 'URL_SUBFOLDER', value: 'DCCMedusaPackager')
                    ]
                )
            }
        }
        stage("HathiValidate"){
            steps{
                build(
                    job: 'OpenSourceProjects/HathiValidate/master',
                    parameters: [
                        string(name: 'PROJECT_NAME', value: 'Hathi Validate'),
                        booleanParam(name: 'TEST_RUN_TOX', value: true),
                        booleanParam(name: 'DEPLOY_DEVPI', value: true),
                        booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                        booleanParam(name: 'DEPLOY_ADD_TAG', value: false),
                        booleanParam(name: 'DEPLOY_HATHI_TOOL_BETA', value: false),
                        booleanParam(name: 'DEPLOY_DOCS', value: false),
                        string(name: 'URL_SUBFOLDER', value: 'hathi_validate')
                    ]
                )
            }
        }

        stage("PackageValidation"){
            steps{
                build(
                    job: 'OpenSourceProjects/PackageValidation/master',
                    parameters: [
                        booleanParam(name: 'TEST_RUN_TOX', value: true),
                        booleanParam(name: 'PACKAGE_CX_FREEZE', value: true),
                        booleanParam(name: 'DEPLOY_DEVPI', value: true),
                        booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                        booleanParam(name: 'UPDATE_DOCS', value: false),
                        string(name: 'URL_SUBFOLDER', value: 'package_qc')
                    ]
                )
            }
        }
        stage("Speedwagon"){
            steps{
                build(
                    job: 'OpenSourceProjects/Speedwagon/master',
                    parameters: [
                        string(name: 'JIRA_ISSUE_VALUE', value: 'PSR-83'),
                        booleanParam(name: 'TEST_RUN_TOX', value: true),
                        booleanParam(name: 'USE_SONARQUBE', value: true),
                        booleanParam(name: 'PACKAGE_WINDOWS_STANDALONE_MSI', value: true),
                        booleanParam(name: 'PACKAGE_WINDOWS_STANDALONE_NSIS', value: false),
                        booleanParam(name: 'PACKAGE_WINDOWS_STANDALONE_ZIP', value: false),
                        booleanParam(name: 'PACKAGE_WINDOWS_STANDALONE_CHOLOCATEY', value: false),
                        booleanParam(name: 'DEPLOY_DEVPI', value: true),
                        booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                        booleanParam(name: 'DEPLOY_ADD_TAG', value: false),
                        booleanParam(name: 'DEPLOY_CHOLOCATEY', value: false),
                        booleanParam(name: 'DEPLOY_HATHI_TOOL_BETA', value: false),
                        booleanParam(name: 'DEPLOY_SCCM', value: false),
                        booleanParam(name: 'DEPLOY_DOCS', value: false),
                        string(name: 'DEPLOY_DOCS_URL_SUBFOLDER', value: 'speedwagon')
                    ]
                )
            }
        }

//         stage("uiucprescon.imagevalidate"){
//             steps{
//             }
//         }

    }


}
