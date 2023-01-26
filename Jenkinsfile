pipeline {
    agent none
    triggers {
      cron '0 0 * * *'
    }
    options {
      disableConcurrentBuilds()
      timeout(time: 1, unit: 'DAYS')
    }
    parameters {
      booleanParam defaultValue: false, description: 'Run nightly build for Tyko',                      name: "BUILD_Tyko"
      booleanParam defaultValue: true,  description: 'Run nightly build for HathiZip',                  name: "BUILD_HathiZip"
      booleanParam defaultValue: true,  description: 'Run nightly build for pyhathiprep',               name: "BUILD_pyhathiprep"
      booleanParam defaultValue: true,  description: 'Run nightly build for uiucprescon.images',        name: "BUILD_uiucprescon_images"
      booleanParam defaultValue: true,  description: 'Run nightly build for uiucprescon.packager',      name: "BUILD_uiucprescon_packager"
      booleanParam defaultValue: true,  description: 'Run nightly build for uiucprescon.imagevalidate', name: "BUILD_uiucprescon_imagevalidate"
      booleanParam defaultValue: true,  description: 'Run nightly build for uiucprescon.build',         name: "BUILD_UIUCPRESCON_BUILD"
      booleanParam defaultValue: true,  description: 'Run nightly build for pyexiv2bind2',              name: "BUILD_pyexiv2bind2"
      booleanParam defaultValue: true,  description: 'Run nightly build for uiucprescon.getmarc2',      name: "BUILD_uiucprescon_getalmarc2"
      booleanParam defaultValue: true,  description: 'Run nightly build for uiucprescon.ocr',           name: "BUILD_uiucprescon_ocr"
      booleanParam defaultValue: true,  description: 'Run nightly build for HathiValidate',             name: "BUILD_HathiValidate"
      booleanParam defaultValue: true,  description: 'Run nightly build for PackageValidation',         name: "BUILD_PackageValidation"
      booleanParam defaultValue: true,  description: 'Run nightly build for speedwagon',                name: "BUILD_speedwagon"
      booleanParam defaultValue: true,  description: 'Run nightly build for getmarcapi',                name: "BUILD_getmarcapi"
      booleanParam defaultValue: true,  description: 'Deploy to Devpi server for testing',              name: "DEPLOY_DEVPI"
      booleanParam defaultValue: true,  description: 'Include Mac builds in pipelines',                 name: "INCLUDE_MAC"

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
                        retry(2){
                            build(
                                job: 'OpenSourceProjects/tyko/master',
                                  parameters: [
                                    booleanParam(name: 'USE_SONARQUBE', value: true),
                                    booleanParam(name: 'TEST_RUN_TOX', value: true),
                                    booleanParam(name: 'DEPLOY_SERVER', value: false),
                                    booleanParam(name: 'DEPLOY_DOCS', value: false)
                                  ]
                                )
                        }
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
                        retry(2){
                            build(
                                job: 'OpenSourceProjects/HathiZip/master',
                                parameters: [
                                    string(name: 'PROJECT_NAME', value: 'HathiTrust Zip for Submit'),
                                    booleanParam(name: 'TEST_RUN_TOX', value: true),
                                    booleanParam(name: 'BUILD_PACKAGES', value: true),
                                    booleanParam(name: 'TEST_PACKAGES', value: true),
                                    booleanParam(name: 'TEST_PACKAGES_ON_MAC', value: params.INCLUDE_MAC),
                                    booleanParam(name: 'PACKAGE_CX_FREEZE', value: true),
                                    booleanParam(name: 'INCLUDE_ARM', value: true),
                                    booleanParam(name: 'DEPLOY_DEVPI', value: params.DEPLOY_DEVPI),
                                    booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                                    booleanParam(name: 'DEPLOY_SCCM', value: false),
                                    booleanParam(name: 'UPDATE_DOCS', value: false)
                                ]
                            )
                        }
                    }
                }
                stage("uiucprescon.build"){
                    options {
                        warnError('uiucprescon.build Build failed')
                    }
                    when{
                        equals expected: true, actual: params.BUILD_UIUCPRESCON_BUILD
                    }
                    steps{
                        retry(2){
                            build(
                                job: 'OpenSourceProjects/uiucprescon.build/main',
                                parameters: [
                                    booleanParam(name: 'TEST_RUN_TOX', value: true),
                                ]
                            )
                        }
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
                        retry(2){
                            build(
                                job: 'OpenSourceProjects/pyhathiprep/master',
                                parameters: [
                                    booleanParam(name: 'TEST_RUN_TOX', value: true),
                                    booleanParam(name: 'DEPLOY_DEVPI', value: params.DEPLOY_DEVPI),
                                    booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                                    string(name: 'URL_SUBFOLDER', value: 'pyhathiprep'),
                                    booleanParam(name: 'DEPLOY_DOCS', value: false),
                                    booleanParam(name: 'DEPLOY_ADD_TAG', value: false)
                                ]
                            )
                        }
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
                        retry(2){
                            build(
                                job: 'OpenSourceProjects/imageprocess/master',
                                parameters: [
                                    booleanParam(name: 'TEST_RUN_TOX', value: true),
                                    booleanParam(name: 'DEPLOY_DEVPI', value: params.DEPLOY_DEVPI),
                                    booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                                    booleanParam(name: 'DEPLOY_ADD_TAG', value: false),
                                    booleanParam(name: 'DEPLOY_DOCS', value: false)
                                ]
                            )
                        }
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
                        retry(2){
                            build(
                                job: 'OpenSourceProjects/Packager/master',
                                parameters: [
                                    booleanParam(name: 'RUN_CHECKS', value: true),
                                    booleanParam(name: 'TEST_RUN_TOX', value: true),
                                    booleanParam(name: 'USE_SONARQUBE', value: true),
                                    booleanParam(name: 'BUILD_PACKAGES', value: true),
                                    booleanParam(name: 'INCLUDE_ARM_LINUX', value: true),
                                    booleanParam(name: 'INCLUDE_ARM_MACOS', value: params.INCLUDE_MAC),
                                    booleanParam(name: 'INCLUDE_X86_64_MACOS', value: params.INCLUDE_MAC),
                                    booleanParam(name: 'DEPLOY_DEVPI', value: params.DEPLOY_DEVPI),
                                    booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                                    booleanParam(name: 'DEPLOY_DOCS', value: false),
                                    string(name: 'DEPLOY_DOCS_URL_SUBFOLDER', value: 'packager')
                                ]
                            )
                        }
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
                        retry(2){
                            build(
                                job: 'OpenSourceProjects/imagevalidate/master',
                                parameters: [
                                    booleanParam(name: 'RUN_CHECKS', value: true),
                                    booleanParam(name: 'TEST_RUN_TOX', value: true),
                                    booleanParam(name: 'BUILD_PACKAGES', value: true),
                                    booleanParam(name: 'TEST_PACKAGES', value: true),
                                    booleanParam(name: 'BUILD_MAC_PACKAGES', value: params.INCLUDE_MAC),
                                    booleanParam(name: 'INCLUDE_ARM', value: true),
                                    booleanParam(name: 'DEPLOY_DEVPI', value: params.DEPLOY_DEVPI),
                                    booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                                    booleanParam(name: 'DEPLOY_DOCS', value: false),
                                    string(name: 'DEPLOY_DOCS_URL_SUBFOLDER', value: 'imagevalidate')
                                ]
                            )
                        }
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
                        retry(2){
                            build(
                                job: 'OpenSourceProjects/pyexiv2bind2/master',
                                parameters: [
                                    booleanParam(name: 'TEST_RUN_TOX', value: true),
                                    booleanParam(name: 'RUN_MEMCHECK', value: true),
                                    booleanParam(name: 'USE_SONARQUBE', value: true),
                                    booleanParam(name: 'DEPLOY_DEVPI', value: params.DEPLOY_DEVPI),
                                    booleanParam(name: 'BUILD_MAC_PACKAGES', value: params.INCLUDE_MAC),
                                    booleanParam(name: 'INCLUDE_ARM', value: true),
                                    booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                                    string(name: 'URL_SUBFOLDER', value: 'py3exiv2bind'),
                                    booleanParam(name: 'DEPLOY_DOCS', value: false)
                                ]
                            )
                        }
                    }
                }
//                 stage("DCCMedusaPackager"){
//                     options {
//                         warnError('DCCMedusaPackager Build failed')
//                     }
//                     when{
//                         equals expected: true, actual: params.BUILD_DCCMedusaPackager
//                     }
//                     steps{
//                         build(
//                             job: 'OpenSourceProjects/DCCMedusaPackager/master',
//                             parameters: [
//                                 string(name: 'PROJECT_NAME', value: 'Medusa Packager'),
//                                 booleanParam(name: 'TEST_RUN_TOX', value: true),
//                                 booleanParam(name: 'PACKAGE_CX_FREEZE', value: true),
//                                 booleanParam(name: 'DEPLOY_DEVPI', value: true),
//                                 booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
//                                 booleanParam(name: 'DEPLOY_SCCM', value: false),
//                                 booleanParam(name: 'UPDATE_DOCS', value: false),
//                                 string(name: 'URL_SUBFOLDER', value: 'DCCMedusaPackager')
//                             ]
//                         )
//                     }
//                 }
                stage("HathiValidate"){
                    options {
                        warnError('HathiValidate Build failed')
                    }
                    when{
                        equals expected: true, actual: params.BUILD_HathiValidate
                    }
                    steps{
                        retry(2){
                            build(
                                job: 'OpenSourceProjects/HathiValidate/master',
                                parameters: [
                                    string(name: 'PROJECT_NAME', value: 'Hathi Validate'),
                                    booleanParam(name: 'TEST_RUN_TOX', value: true),
                                    booleanParam(name: 'BUILD_PACKAGES', value: true),
                                    booleanParam(name: 'TEST_PACKAGES_ON_MAC', value: params.INCLUDE_MAC),
                                    booleanParam(name: 'DEPLOY_DEVPI', value: params.DEPLOY_DEVPI),
                                    booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                                    booleanParam(name: 'DEPLOY_ADD_TAG', value: false),
                                    booleanParam(name: 'DEPLOY_HATHI_TOOL_BETA', value: false),
                                    booleanParam(name: 'DEPLOY_DOCS', value: false),
                                    string(name: 'URL_SUBFOLDER', value: 'hathi_validate')
                                ]
                            )
                        }
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
                        retry(2){
                            build(
                                job: 'OpenSourceProjects/PackageValidation/master',
                                parameters: [
                                    booleanParam(name: 'TEST_RUN_TOX', value: true),
                                    booleanParam(name: 'PACKAGE_CX_FREEZE', value: true),
                                    booleanParam(name: 'DEPLOY_DEVPI', value: params.DEPLOY_DEVPI),
                                    booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                                    booleanParam(name: 'UPDATE_DOCS', value: false),
                                    string(name: 'URL_SUBFOLDER', value: 'package_qc')
                                ]
                            )
                        }
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
                        retry(2){
                            build(
                                job: 'OpenSourceProjects/Speedwagon/master',
                                parameters: [
                                    string(name: 'JIRA_ISSUE_VALUE', value: 'PSR-83'),
                                    booleanParam(name: 'TEST_RUN_TOX', value: true),
                                    booleanParam(name: 'USE_SONARQUBE', value: true),
                                    booleanParam(name: 'PACKAGE_WINDOWS_STANDALONE_MSI', value: true),
                                    booleanParam(name: 'PACKAGE_WINDOWS_STANDALONE_NSIS', value: true),
                                    booleanParam(name: 'PACKAGE_WINDOWS_STANDALONE_ZIP', value: true),
                                    booleanParam(name: 'DEPLOY_DMG', value: false),
                                    booleanParam(name: 'PACKAGE_MAC_OS_STANDALONE_DMG', value: params.INCLUDE_MAC),
                                    booleanParam(name: 'TEST_PACKAGES_ON_MAC', value: params.INCLUDE_MAC),
                                    booleanParam(name: 'BUILD_CHOCOLATEY_PACKAGE', value: true),
                                    booleanParam(name: 'DEPLOY_DEVPI', value: params.DEPLOY_DEVPI),
                                    booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                                    booleanParam(name: 'DEPLOY_CHOLOCATEY', value: false),
                                    booleanParam(name: 'DEPLOY_HATHI_TOOL_BETA', value: false),
                                    booleanParam(name: 'DEPLOY_SCCM', value: false),
                                    booleanParam(name: 'DEPLOY_DOCS', value: false),
                                    string(name: 'DEPLOY_DOCS_URL_SUBFOLDER', value: 'speedwagon')
                                ]
                            )
                        }
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
                        retry(2){
                            build(
                                job: 'OpenSourceProjects/uiucprescon.getalmarc2/master',
                                parameters: [
                                    booleanParam(name: 'TEST_RUN_TOX', value: true),
                                    booleanParam(name: 'BUILD_PACKAGES', value: true),
                                    booleanParam(name: 'TEST_PACKAGES_ON_MAC', value: params.INCLUDE_MAC),
                                    booleanParam(name: 'BUILD_CHOCOLATEY_PACKAGE', value: true),
                                    booleanParam(name: 'DEPLOY_DEVPI', value: params.DEPLOY_DEVPI),
                                    booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false)
                                ]
                            )
                        }
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
                        retry(2){
                            build(
                                job: 'OpenSourceProjects/Tesseract_Glue/master',
                                parameters: [
                                    booleanParam(name: 'RUN_CHECKS', value: true),
                                    booleanParam(name: 'TEST_RUN_TOX', value: true),
                                    booleanParam(name: 'USE_SONARQUBE', value: true),
                                    booleanParam(name: 'BUILD_PACKAGES', value: true),
                                    booleanParam(name: 'INCLUDE_ARM', value: true),
                                    booleanParam(name: 'BUILD_MAC_PACKAGES', value: params.INCLUDE_MAC),
                                    booleanParam(name: 'TEST_PACKAGES', value: true),
                                    booleanParam(name: 'BUILD_MANYLINUX_PACKAGES', value: true),
                                    booleanParam(name: 'DEPLOY_DEVPI', value: params.DEPLOY_DEVPI),
                                    booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                                    booleanParam(name: 'DEPLOY_DOCS', value: false)
                                ]
                            )
                        }
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
                        retry(2){
                            build(
                                job: 'OpenSourceProjects/getmarcapi/master',
                                parameters: [
                                    booleanParam(name: 'TEST_RUN_TOX', value: true),
                                    booleanParam(name: 'USE_SONARQUBE', value: true),
                                    booleanParam(name: 'BUILD_PACKAGES', value: true),
                                    booleanParam(name: 'DEPLOY_DEVPI', value: params.DEPLOY_DEVPI),
                                    booleanParam(name: 'DEPLOY_DEVPI_PRODUCTION', value: false),
                                    booleanParam(name: 'DEPLOY_DOCS', value: false)
                                ]
                            )
                        }
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
