pipeline {
    agent none
    triggers {
      cron '0 0 * * *'
    }
    options {
      disableConcurrentBuilds()
    }
    parameters {
      booleanParam defaultValue: true, description: 'Run nightly build for Tyko',                      name: "BUILD_Tyko"
      booleanParam defaultValue: true, description: 'Run nightly build for HathiZip',                  name: "BUILD_HathiZip"
      booleanParam defaultValue: true, description: 'Run nightly build for pyhathiprep',               name: "BUILD_pyhathiprep"
      booleanParam defaultValue: true, description: 'Run nightly build for HT_checksum_update',        name: "BUILD_HT_checksum_update"
      booleanParam defaultValue: true, description: 'Run nightly build for uiucprescon.images',        name: "BUILD_uiucprescon_images"
      booleanParam defaultValue: true, description: 'Run nightly build for uiucprescon.packager',      name: "BUILD_uiucprescon_packager"
      booleanParam defaultValue: true, description: 'Run nightly build for uiucprescon.imagevalidate', name: "BUILD_uiucprescon_imagevalidate"
      booleanParam defaultValue: true, description: 'Run nightly build for pyexiv2bind2',              name: "BUILD_pyexiv2bind2"
      booleanParam defaultValue: true, description: 'Run nightly build for uiucprescon.getmarc2',      name: "BUILD_uiucprescon_getalmarc2"
      booleanParam defaultValue: true, description: 'Run nightly build for uiucprescon.ocr',           name: "BUILD_uiucprescon_ocr"
      booleanParam defaultValue: true, description: 'Run nightly build for DCCMedusaPackager',         name: "BUILD_DCCMedusaPackager"
      booleanParam defaultValue: true, description: 'Run nightly build for HathiValidate',             name: "BUILD_HathiValidate"
      booleanParam defaultValue: true, description: 'Run nightly build for PackageValidation',         name: "BUILD_PackageValidation"
      booleanParam defaultValue: true, description: 'Run nightly build for speedwagon',                name: "BUILD_speedwagon"
      booleanParam defaultValue: true, description: 'Run nightly build for getmarcapi',                name: "BUILD_getmarcapi"

    }
    stages{
        stage("Run Nightly builds"){
            parallel{
                stage("Tyko"){
                    options {
                        warnError('Tyko Build failed')
                    }
                    when{
                        equals expected: true, actual: params.BUILD_Tyko
                    }
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
                    options {
                        warnError('HathiZip Build failed')
                    }
                    when{
                        equals expected: true, actual: params.BUILD_HathiZip
                    }
                    steps{
                        build(
                            job: 'OpenSourceProjects/HathiZip/master',
                            parameters: [
                                string(name: 'PROJECT_NAME', value: 'HathiTrust Zip for Submit'),
                                booleanParam(name: 'TEST_RUN_TOX', value: true),
                                booleanParam(name: 'TEST_PACKAGES', value: true),
                                booleanParam(name: 'TEST_PACKAGES_ON_MAC', value: true),
                                booleanParam(name: 'PACKAGE_CX_FREEZE', value: true),
                                booleanParam(name: 'DEPLOY_DEVPI', value: true),
                                booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                                booleanParam(name: 'DEPLOY_SCCM', value: false),
                                booleanParam(name: 'UPDATE_DOCS', value: false)
                            ]
                        )
                    }
                }
                stage("pyhathiprep"){
                    options {
                        warnError('pyhathiprep Build failed')
                    }
                    when{
                        equals expected: true, actual: params.BUILD_pyhathiprep
                    }
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
                    options {
                        warnError('HT_checksum_update Build failed')
                    }
                    when{
                        equals expected: true, actual: params.BUILD_HT_checksum_update
                    }
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
                    options {
                        warnError('HT_checksum_update Build failed')
                    }
                    when{
                        equals expected: true, actual: params.BUILD_uiucprescon_images
                    }
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
                    options {
                        warnError('uiucprescon.packager Build failed')
                    }
                    when{
                        equals expected: true, actual: params.BUILD_uiucprescon_packager
                    }
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
                    options {
                        warnError('uiucprescon.imagevalidate Build failed')
                    }
                    when{
                        equals expected: true, actual: params.BUILD_uiucprescon_imagevalidate
                    }
                    steps{
                        build(
                            job: 'OpenSourceProjects/imagevalidate/master',
                            parameters: [
                                booleanParam(name: 'RUN_CHECKS', value: true),
                                booleanParam(name: 'TEST_RUN_TOX', value: true),
                                booleanParam(name: 'BUILD_PACKAGES', value: true),
                                booleanParam(name: 'TEST_PACKAGES', value: true),
                                booleanParam(name: 'BUILD_MAC_PACKAGES', value: true),
                                booleanParam(name: 'DEPLOY_DEVPI', value: true),
                                booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                                booleanParam(name: 'DEPLOY_DOCS', value: false),
                                string(name: 'DEPLOY_DOCS_URL_SUBFOLDER', value: 'imagevalidate')
                            ]
                        )
                    }
                }
                stage("pyexiv2bind2"){
                    options {
                        warnError('pyexiv2bind2 Build failed')
                    }
                    when{
                        equals expected: true, actual: params.BUILD_pyexiv2bind2
                    }
                    steps{
                        build(
                            job: 'OpenSourceProjects/pyexiv2bind2/master',
                            parameters: [
                                booleanParam(name: 'TEST_RUN_TOX', value: true),
                                booleanParam(name: 'USE_SONARQUBE', value: true),
                                booleanParam(name: 'DEPLOY_DEVPI', value: true),
                                booleanParam(name: 'BUILD_MAC_PACKAGES', value: true),
                                booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                                string(name: 'URL_SUBFOLDER', value: 'py3exiv2bind'),
                                booleanParam(name: 'DEPLOY_DOCS', value: false)
                            ]
                        )
                    }
                }
                stage("DCCMedusaPackager"){
                    options {
                        warnError('DCCMedusaPackager Build failed')
                    }
                    when{
                        equals expected: true, actual: params.BUILD_DCCMedusaPackager
                    }
                    steps{
                        build(
                            job: 'OpenSourceProjects/DCCMedusaPackager/master',
                            parameters: [
                                string(name: 'PROJECT_NAME', value: 'Medusa Packager'),
                                booleanParam(name: 'TEST_RUN_TOX', value: true),
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
                    options {
                        warnError('HathiValidate Build failed')
                    }
                    when{
                        equals expected: true, actual: params.BUILD_HathiValidate
                    }
                    steps{
                        build(
                            job: 'OpenSourceProjects/HathiValidate/master',
                            parameters: [
                                string(name: 'PROJECT_NAME', value: 'Hathi Validate'),
                                booleanParam(name: 'TEST_RUN_TOX', value: true),
                                booleanParam(name: 'BUILD_PACKAGES', value: true),
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
                    options {
                        warnError('PackageValidation Build failed')
                    }
                    when{
                        equals expected: true, actual: params.BUILD_PackageValidation
                    }
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
                    options {
                        warnError('Speedwagon Build failed')
                    }
                    when{
                        equals expected: true, actual: params.BUILD_speedwagon
                    }
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
                                booleanParam(name: 'TEST_MAC_PACKAGES', value: true),
                                booleanParam(name: 'BUILD_CHOCOLATEY_PACKAGE', value: true),
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
                stage("uiucprescon.getalmarc2"){
                    options {
                        warnError('uiucprescon.getalmarc2 Build failed')
                    }
                    when{
                        equals expected: true, actual: params.BUILD_uiucprescon_getalmarc2
                    }
                    steps{
                        build(
                            job: 'OpenSourceProjects/uiucprescon.getalmarc2/master',
                            parameters: [
                                booleanParam(name: 'TEST_RUN_TOX', value: true),
                                booleanParam(name: 'BUILD_PACKAGES', value: true),
                                booleanParam(name: 'TEST_PACKAGES_ON_MAC', value: true),
                                booleanParam(name: 'BUILD_CHOCOLATEY_PACKAGE', value: true),
                                booleanParam(name: 'DEPLOY_DEVPI', value: true),
                                booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false)
                            ]
                        )
                    }
                }
                 stage("uiucprescon.ocr"){
                    options {
                        warnError('uiucprescon.ocr Build failed')
                    }
                    when{
                        equals expected: true, actual: params.BUILD_uiucprescon_ocr
                    }
                    steps{
                        build(
                            job: 'OpenSourceProjects/Tesseract_Glue/master',
                            parameters: [
                                booleanParam(name: 'RUN_CHECKS', value: true),
                                booleanParam(name: 'TEST_RUN_TOX', value: true),
                                booleanParam(name: 'USE_SONARQUBE', value: true),
                                booleanParam(name: 'BUILD_PACKAGES', value: true),
                                booleanParam(name: 'BUILD_MAC_PACKAGES', value: true),
                                booleanParam(name: 'TEST_PACKAGES', value: true),
                                booleanParam(name: 'DEPLOY_DEVPI', value: true),
                                booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                                booleanParam(name: 'DEPLOY_DOCS', value: false)
                            ]
                        )
                    }
                }
                stage("getmarcapi"){
                    options {
                        warnError('getmarcapi Build failed')
                    }
                    when{
                        equals expected: true, actual: params.BUILD_getmarcapi
                    }
                    steps{
                        build(
                            job: 'OpenSourceProjects/getmarcapi/master',
                            parameters: [
                                booleanParam(name: 'TEST_RUN_TOX', value: true),
                                booleanParam(name: 'USE_SONARQUBE', value: true),
                                booleanParam(name: 'BUILD_PACKAGES', value: true),
                                booleanParam(name: 'DEPLOY_DEVPI', value: true),
                                booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                                booleanParam(name: 'DEPLOY_DOCS', value: false)
                            ]
                        )
                    }
                }
            }
        }
//         stage("uiucprescon.imagevalidate"){
//             steps{
//             }
//         }

    }


}
