
isTriggeredByTimer = !currentBuild.getBuildCauses('hudson.triggers.TimerTrigger$TimerTriggerCause').isEmpty()
isTriggeredByUser = !currentBuild.getBuildCauses('hudson.model.Cause$UserIdCause').isEmpty()

armMacIsAvailable = nodesByLabel('mac && arm64').size() > 0
x86_64MacIsAvailable = nodesByLabel('mac && x86_64').size() > 0
x86_64WindowsIsAvailable = nodesByLabel('windows && x86_64').size() > 0

pipeline {
    agent none
    options {
      disableConcurrentBuilds()
      timeout(time: 1, unit: 'DAYS')
    }
    parameters {
      booleanParam defaultValue: true,  description: 'Audit homebrew-uiucprescon formulas and casks',   name: "AUDIT_HOMEBREW"
      booleanParam defaultValue: true,  description: 'Run nightly build for HathiZip',                  name: "BUILD_HathiZip"
      booleanParam defaultValue: true,  description: 'Run nightly build for Cloudwagon',                name: "BUILD_cloudwagon"
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
      booleanParam defaultValue: true,  description: 'Run nightly build for uiucpreson_workflows',      name: "BUILD_uiupreson_workflows"
      booleanParam defaultValue: true,  description: 'Run nightly build for galatea',                   name: "BUILD_galatea"
      booleanParam defaultValue: true,  description: 'Run nightly build for Tripwire',                  name: "BUILD_Tripwire"
      booleanParam defaultValue: true,  description: 'Run nightly build for getmarcapi',                name: "BUILD_getmarcapi"
      booleanParam defaultValue: true,  description: 'Include Mac builds in pipelines',                 name: "INCLUDE_MAC"
      booleanParam defaultValue: true,  description: 'Include Linux ARM builds in pipelines',           name: "INCLUDE_LINUX_ARM"
      booleanParam defaultValue: true,  description: 'Include Windows x86_64 builds in pipelines',      name: "INCLUDE_WINDOWS_X86_64"
      booleanParam defaultValue: true,  description: 'Send test data to Sonarqube',                     name: "USE_SONARQUBE"

    }
    stages{
        stage("Audit Homebrew Tap"){
            options {
                warnError('homebrew-uiucprescon failed audit')
            }
            when{
                equals expected: true, actual: params.AUDIT_HOMEBREW
            }
            steps{
                build(
                    job: 'open source/Homebrew tap for uiuclibrary uiucprescon/master',
                    parameters: [
                        booleanParam(name: 'AUDIT_FORMULA', value: true),
                        booleanParam(name: 'AUDIT_FORMULA_ONLINE_OPTION', value: true),
                        booleanParam(name: 'BOTTLE_FORMULA', value: false),
                        booleanParam(name: 'BOTTLE_UPLOAD', value: false),
                    ]
                )
            }

        }
        stage("Run Nightly builds"){
            parallel{
                stage("Cloudwagon"){
                    options {
                        warnError('Cloudwagon Build failed')
                    }
                    when{
                        equals expected: true, actual: params.BUILD_cloudwagon
                    }
                    steps{
                        retry(2){
                            build(
                                job: 'open source/cloudwagon/main',
                                parameters: [
                                    booleanParam(name: 'RUN_CHECKS', value: true),
                                    booleanParam(name: 'USE_SONARQUBE', value: true),
                                    booleanParam(name: 'TEST_RUN_TOX', value: true),
                                    booleanParam(name: 'BUILD_DOCKER', value: true),
                                    booleanParam(name: 'PACKAGE', value: true),
                                    booleanParam(name: 'PUBLISH_DOCKER', value: false),
                                    booleanParam(name: 'INCLUDE-arm64', value: params.INCLUDE_LINUX_ARM),
                                    booleanParam(name: 'INCLUDE-x86_64', value: true),
                                ]
                            )
                        }
                    }
                }
                stage("Galatea"){
                    options {
                        warnError('Galatea Build failed')
                        retry(2)
                    }
                    when{
                        equals expected: true, actual: params.BUILD_galatea
                    }
                    steps{
                        build(
                            job: 'open source/Galatea/main',
                            parameters: [
                                booleanParam(name: 'RUN_CHECKS', value: true),
                                booleanParam(name: 'TEST_RUN_TOX', value: true),
                                booleanParam(name: 'BUILD_PACKAGES', value: true),
                                booleanParam(name: 'TEST_PACKAGES', value: true),
                                booleanParam(name: 'INCLUDE_LINUX-X86_64', value: true),
                                booleanParam(name: 'INCLUDE_LINUX-ARM64', value: true),
                                booleanParam(name: 'INCLUDE_MACOS-X86_64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (x86_64MacIsAvailable && isTriggeredByTimer)),
                                booleanParam(name: 'INCLUDE_MACOS-ARM64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (armMacIsAvailable && isTriggeredByTimer)),
                                booleanParam(name: 'INCLUDE_WINDOWS-X86_64', value: (params.INCLUDE_WINDOWS_X86_64 && isTriggeredByUser) || (isTriggeredByTimer && x86_64WindowsIsAvailable)),
                                booleanParam(name: 'USE_SONARQUBE', value: params.USE_SONARQUBE),
                                booleanParam(name: 'PACKAGE_MAC_OS_STANDALONE_X86_64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (x86_64MacIsAvailable && isTriggeredByTimer)),
                                booleanParam(name: 'PACKAGE_MAC_OS_STANDALONE_ARM64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (armMacIsAvailable && isTriggeredByTimer)),
                                booleanParam(name: 'PACKAGE_STANDALONE_WINDOWS_INSTALLER', value: (params.INCLUDE_WINDOWS_X86_64 && isTriggeredByUser) || (isTriggeredByTimer && x86_64WindowsIsAvailable)),
                                booleanParam(name: 'DEPLOY_STANDALONE_PACKAGERS', value: false),
                                
                            ]
                        )
                    }
                }
                stage("Tripwire"){
                    options {
                        warnError('Tripwire Build failed')
                        retry(2)
                    }
                    when{
                        equals expected: true, actual: params.BUILD_Tripwire
                    }
                    steps{
                        build(
                            job: 'open source/tripwire/main',
                            parameters: [
                                booleanParam(name: 'RUN_CHECKS', value: true),
                                booleanParam(name: 'BUILD_PACKAGES', value: true),
                                booleanParam(name: 'TEST_PACKAGES', value: true),
                                booleanParam(name: 'INCLUDE_LINUX-X86_64', value: true),
                                booleanParam(name: 'INCLUDE_LINUX-ARM64', value: true),
                                booleanParam(name: 'INCLUDE_MACOS-X86_64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (x86_64MacIsAvailable && isTriggeredByTimer)),
                                booleanParam(name: 'INCLUDE_MACOS-ARM64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (armMacIsAvailable && isTriggeredByTimer)),
                                booleanParam(name: 'INCLUDE_WINDOWS-X86_64', value: (params.INCLUDE_WINDOWS_X86_64 && isTriggeredByUser) || (isTriggeredByTimer && x86_64WindowsIsAvailable)),
                                // booleanParam(name: 'USE_SONARQUBE', value: params.USE_SONARQUBE),
                                booleanParam(name: 'PACKAGE_STANDALONE_WINDOWS_INSTALLER', value: (params.INCLUDE_WINDOWS_X86_64 && isTriggeredByUser) || (isTriggeredByTimer && x86_64WindowsIsAvailable)),
                                booleanParam(name: 'PACKAGE_MAC_OS_STANDALONE_X86_64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (x86_64MacIsAvailable && isTriggeredByTimer)),
                                booleanParam(name: 'PACKAGE_MAC_OS_STANDALONE_ARM64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (armMacIsAvailable && isTriggeredByTimer)),
                                booleanParam(name: 'DEPLOY_PYPI', value: false),
                                booleanParam(name: 'DEPLOY_STANDALONE_PACKAGERS', value: false),    
                            ]
                        )
                    }
                }
                stage("HathiZip"){
                    options {
                        warnError('HathiZip Build failed')
                        retry(2)
                    }
                    when{
                        equals expected: true, actual: params.BUILD_HathiZip
                    }
                    steps{
                        build(
                            job: 'open source/HathiZip/master',
                            parameters: [
                                string(name: 'PROJECT_NAME', value: 'HathiTrust Zip for Submit'),
                                booleanParam(name: 'TEST_RUN_TOX', value: true),
                                booleanParam(name: 'BUILD_PACKAGES', value: true),
                                booleanParam(name: 'TEST_PACKAGES', value: true),
                                booleanParam(name: 'INCLUDE_LINUX-ARM64', value: params.INCLUDE_LINUX_ARM),
                                booleanParam(name: 'INCLUDE_LINUX-X86_64', value: true),
                                booleanParam(name: 'INCLUDE_MACOS-X86_64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (x86_64MacIsAvailable && isTriggeredByTimer)),
                                booleanParam(name: 'INCLUDE_MACOS-ARM64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (armMacIsAvailable && isTriggeredByTimer)),
                                booleanParam(name: 'INCLUDE_WINDOWS-X86_64', value: (params.INCLUDE_WINDOWS_X86_64 && isTriggeredByUser) || (isTriggeredByTimer && x86_64WindowsIsAvailable)),
                                booleanParam(name: 'USE_SONARQUBE', value: params.USE_SONARQUBE),
                            ]
                        )
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
                                job: 'open source/uiucprescon build/main',
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
                        retry(2)
                    }
                    when{
                        equals expected: true, actual: params.BUILD_pyhathiprep
                    }
                    steps{

                        build(
                            job: 'open source/pyhathiprep/master',
                            parameters: [
                                booleanParam(name: 'TEST_RUN_TOX', value: true),
                                booleanParam(name: 'BUILD_PACKAGES', value: true),
                                booleanParam(name: 'TEST_PACKAGES', value: true),
                                booleanParam(name: 'INCLUDE_MACOS-ARM64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (armMacIsAvailable && isTriggeredByTimer)),
                                booleanParam(name: 'INCLUDE_MACOS-X86_64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (x86_64MacIsAvailable && isTriggeredByTimer)),
                                booleanParam(name: 'INCLUDE_LINUX-ARM64', value: params.INCLUDE_LINUX_ARM),
                                booleanParam(name: 'INCLUDE_LINUX-X86_64', value: true),
                                booleanParam(name: 'INCLUDE_WINDOWS-X86_64', value: (params.INCLUDE_WINDOWS_X86_64 && isTriggeredByUser) || (isTriggeredByTimer && x86_64WindowsIsAvailable)),
                                booleanParam(name: 'USE_SONARQUBE', value: params.USE_SONARQUBE),
                                booleanParam(name: 'DEPLOY_DOCS', value: false),
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
                        retry(2){
                            build(
                                job: 'open source/uiucprescon.images/master',
                                parameters: [
                                    booleanParam(name: 'TEST_RUN_TOX', value: true),
                                    booleanParam(name: 'USE_SONARQUBE', value: params.USE_SONARQUBE),
                                    booleanParam(name: 'BUILD_PACKAGES', value: true),
                                    booleanParam(name: 'TEST_PACKAGES', value: true),
                                    booleanParam(name: 'INCLUDE_MACOS-ARM64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (armMacIsAvailable && isTriggeredByTimer)),
                                    booleanParam(name: 'INCLUDE_MACOS-X86_64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (x86_64MacIsAvailable && isTriggeredByTimer)),
                                    booleanParam(name: 'INCLUDE_LINUX-ARM64', value: params.INCLUDE_LINUX_ARM),
                                    booleanParam(name: 'INCLUDE_LINUX-X86_64', value: true),
                                    booleanParam(name: 'INCLUDE_WINDOWS-X86_64', value: (params.INCLUDE_WINDOWS_X86_64 && isTriggeredByUser) || (isTriggeredByTimer && x86_64WindowsIsAvailable)),
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
                                job: 'open source/uiucprescon.packager/master',
                                parameters: [
                                    booleanParam(name: 'RUN_CHECKS', value: true),
                                    booleanParam(name: 'TEST_RUN_TOX', value: true),
                                    booleanParam(name: 'USE_SONARQUBE', value: params.USE_SONARQUBE),
                                    booleanParam(name: 'BUILD_PACKAGES', value: true),
                                    booleanParam(name: 'INCLUDE_LINUX-X86_64', value: true),
                                    booleanParam(name: 'INCLUDE_LINUX-ARM64', value: params.INCLUDE_LINUX_ARM),
                                    booleanParam(name: 'INCLUDE_MACOS-ARM64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (armMacIsAvailable && isTriggeredByTimer)),
                                    booleanParam(name: 'INCLUDE_MACOS-X86_64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (x86_64MacIsAvailable && isTriggeredByTimer)),
                                    booleanParam(name: 'INCLUDE_WINDOWS-X86_64', value: (params.INCLUDE_WINDOWS_X86_64 && isTriggeredByUser) || (isTriggeredByTimer && x86_64WindowsIsAvailable)),
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
                                job: 'open source/uiucprescon.imagevalidate/master',
                                parameters: [
                                    booleanParam(name: 'RUN_CHECKS', value: true),
                                    booleanParam(name: 'TEST_RUN_TOX', value: true),
                                    booleanParam(name: 'BUILD_PACKAGES', value: true),
                                    booleanParam(name: 'TEST_PACKAGES', value: true),
                                    booleanParam(name: 'USE_SONARQUBE', value: params.USE_SONARQUBE),
                                    booleanParam(name: 'INCLUDE_MACOS_ARM', value: (params.INCLUDE_MAC && isTriggeredByUser) || (armMacIsAvailable && isTriggeredByTimer)),
                                    booleanParam(name: 'INCLUDE_MACOS_X86_64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (x86_64MacIsAvailable && isTriggeredByTimer)),
                                    booleanParam(name: 'INCLUDE_LINUX_ARM', value: params.INCLUDE_LINUX_ARM),
                                    booleanParam(name: 'INCLUDE_LINUX_X86_64', value: true),
                                    booleanParam(name: 'INCLUDE_WINDOWS_X86_64', value: (params.INCLUDE_WINDOWS_X86_64 && isTriggeredByUser) || (isTriggeredByTimer && x86_64WindowsIsAvailable)),
                                    booleanParam(name: 'DEPLOY_DOCS', value: false),
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
                                job: 'open source/pyexiv2bind2/master',
                                parameters: [
                                    booleanParam(name: 'TEST_RUN_TOX', value: true),
                                    booleanParam(name: 'RUN_MEMCHECK', value: true),
                                    booleanParam(name: 'USE_SONARQUBE', value: params.USE_SONARQUBE),
                                    booleanParam(name: 'BUILD_PACKAGES', value: true),
                                    booleanParam(name: 'INCLUDE_MACOS_ARM', value: (params.INCLUDE_MAC && isTriggeredByUser) || (armMacIsAvailable && isTriggeredByTimer)),
                                    booleanParam(name: 'INCLUDE_MACOS_X86_64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (x86_64MacIsAvailable && isTriggeredByTimer)),
                                    booleanParam(name: 'INCLUDE_LINUX_ARM', value: params.INCLUDE_LINUX_ARM),
                                    booleanParam(name: 'INCLUDE_LINUX_X86_64', value: true),
                                    booleanParam(name: 'INCLUDE_WINDOWS_X86_64', value: (params.INCLUDE_WINDOWS_X86_64 && isTriggeredByUser) || (isTriggeredByTimer && x86_64WindowsIsAvailable)),
                                    booleanParam(name: 'DEPLOY_DOCS', value: false)
                                ]
                            )
                        }
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
                        retry(2){
                            build(
                                job: 'open source/Hathi Validate/master',
                                parameters: [
                                    string(name: 'PROJECT_NAME', value: 'Hathi Validate'),
                                    booleanParam(name: 'TEST_RUN_TOX', value: true),
                                    booleanParam(name: 'BUILD_PACKAGES', value: true),
                                    booleanParam(name: 'USE_SONARQUBE', value: params.USE_SONARQUBE),
                                    booleanParam(name: 'INCLUDE_LINUX-ARM64', value: params.INCLUDE_LINUX_ARM),
                                    booleanParam(name: 'INCLUDE_LINUX-X86_64', value: true),
                                    booleanParam(name: 'INCLUDE_MACOS-ARM64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (armMacIsAvailable && isTriggeredByTimer)),
                                    booleanParam(name: 'INCLUDE_MACOS-X86_64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (x86_64MacIsAvailable && isTriggeredByTimer)),
                                    booleanParam(name: 'INCLUDE_WINDOWS-X86_64', value: (params.INCLUDE_WINDOWS_X86_64 && isTriggeredByUser) || (isTriggeredByTimer && x86_64WindowsIsAvailable)),
                                    booleanParam(name: 'DEPLOY_HATHI_TOOL_BETA', value: false),
                                    booleanParam(name: 'DEPLOY_DOCS', value: false)
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
                                job: 'open source/PackageValidation/master',
                                parameters: [
                                    booleanParam(name: 'TEST_RUN_TOX', value: true),
                                    booleanParam(name: 'BUILD_PACKAGES', value: true),
                                    booleanParam(name: 'TEST_PACKAGES', value: true),
                                    booleanParam(name: 'INCLUDE_MACOS_ARM', value: (params.INCLUDE_MAC && isTriggeredByUser) || (armMacIsAvailable && isTriggeredByTimer)),
                                    booleanParam(name: 'INCLUDE_MACOS_X86_64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (x86_64MacIsAvailable && isTriggeredByTimer)),
                                    booleanParam(name: 'INCLUDE_LINUX_ARM', value: params.INCLUDE_LINUX_ARM),
                                    booleanParam(name: 'INCLUDE_LINUX_X86_64', value: true),
                                    booleanParam(name: 'INCLUDE_WINDOWS_X86_64', value: (params.INCLUDE_WINDOWS_X86_64 && isTriggeredByUser) || (isTriggeredByTimer && x86_64WindowsIsAvailable)),
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
                                job: 'open source/speedwagon/main',
                                parameters: [
                                    booleanParam(name: 'RUN_CHECKS', value: true),
                                    booleanParam(name: 'USE_SONARQUBE', value: params.USE_SONARQUBE),
                                    booleanParam(name: 'TEST_RUN_TOX', value: true),
                                    booleanParam(name: 'BUILD_PACKAGES', value: true),
                                    booleanParam(name: 'INCLUDE_LINUX-ARM64', value: params.INCLUDE_LINUX_ARM),
                                    booleanParam(name: 'INCLUDE_LINUX-X86_64', value: true),
                                    booleanParam(name: 'INCLUDE_MACOS-ARM64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (armMacIsAvailable && isTriggeredByTimer)),
                                    booleanParam(name: 'INCLUDE_MACOS-X86_64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (x86_64MacIsAvailable && isTriggeredByTimer)),
                                    booleanParam(name: 'INCLUDE_WINDOWS-X86_64', value: (params.INCLUDE_WINDOWS_X86_64 && isTriggeredByUser) || (isTriggeredByTimer && x86_64WindowsIsAvailable)),
                                    booleanParam(name: 'TEST_PACKAGES', value: true),
                                    booleanParam(name: 'DEPLOY_PYPI', value: false),
                                    booleanParam(name: 'DEPLOY_DOCS', value: false),
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
                                job: 'open source/uiucprescon.getmarc2/master',
                                parameters: [
                                    booleanParam(name: 'TEST_RUN_TOX', value: true),
                                    booleanParam(name: 'BUILD_PACKAGES', value: true),
                                    booleanParam(name: 'USE_SONARQUBE', value: params.USE_SONARQUBE),
                                    booleanParam(name: 'INCLUDE_MACOS-ARM64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (armMacIsAvailable && isTriggeredByTimer)),
                                    booleanParam(name: 'INCLUDE_MACOS-X86_64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (x86_64MacIsAvailable && isTriggeredByTimer)),
                                    booleanParam(name: 'INCLUDE_LINUX-ARM64', value: params.INCLUDE_LINUX_ARM),
                                    booleanParam(name: 'INCLUDE_LINUX-X86_64', value: params.INCLUDE_LINUX_ARM),
                                    booleanParam(name: 'INCLUDE_WINDOWS_X86_64', value: (params.INCLUDE_WINDOWS_X86_64 && isTriggeredByUser) || (isTriggeredByTimer && x86_64WindowsIsAvailable)),
                                    booleanParam(name: 'BUILD_CHOCOLATEY_PACKAGE', value: (params.INCLUDE_WINDOWS_X86_64 && isTriggeredByUser) || (isTriggeredByTimer && x86_64WindowsIsAvailable)),
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
                                job: 'open source/uiucprescon.ocr/master',
                                parameters: [
                                    booleanParam(name: 'RUN_CHECKS', value: true),
                                    booleanParam(name: 'TEST_RUN_TOX', value: true),
                                    booleanParam(name: 'USE_SONARQUBE', value: params.USE_SONARQUBE),
                                    booleanParam(name: 'BUILD_PACKAGES', value: true),
                                    booleanParam(name: 'INCLUDE_MACOS_ARM', value: (params.INCLUDE_MAC && isTriggeredByUser) || (armMacIsAvailable && isTriggeredByTimer)),
                                    booleanParam(name: 'INCLUDE_MACOS_X86_64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (x86_64MacIsAvailable && isTriggeredByTimer)),
                                    booleanParam(name: 'INCLUDE_LINUX_ARM', value: params.INCLUDE_LINUX_ARM),
                                    booleanParam(name: 'INCLUDE_LINUX_X86_64', value: true),
                                    booleanParam(name: 'INCLUDE_WINDOWS_X86_64', value: (params.INCLUDE_WINDOWS_X86_64 && isTriggeredByUser) || (isTriggeredByTimer && x86_64WindowsIsAvailable)),
                                    booleanParam(name: 'TEST_PACKAGES', value: true),
                                    booleanParam(name: 'DEPLOY_DOCS', value: false)
                                ]
                            )
                        }
                    }
                }
                stage("getmarcapi"){
                    options {
                        warnError('getmarcapi Build failed')
                        retry(2)
                    }
                    when{
                        equals expected: true, actual: params.BUILD_getmarcapi
                    }
                    steps{
                        build(
                                job: 'open source/getmarcapi/master',
                                parameters: [
                                    booleanParam(name: 'TEST_RUN_TOX', value: true),
                                    booleanParam(name: 'USE_SONARQUBE', value: params.USE_SONARQUBE),
                                    booleanParam(name: 'BUILD_PACKAGES', value: true),
                                    booleanParam(name: 'INCLUDE_LINUX-ARM64', value: params.INCLUDE_LINUX_ARM),
                                    booleanParam(name: 'INCLUDE_LINUX-X86_64', value: true),
                                    booleanParam(name: 'DEPLOY_DOCS', value: false)
                                ]
                        )
                    }
                }
                stage("Uiuc Preson Workflows (speedwagon)"){
                    options {
                        warnError('Uiuc Preson Workflows Build failed')
                        retry(2)
                    }
                    when{
                        equals expected: true, actual: params.BUILD_uiupreson_workflows
                    }
                    steps{
                        build(
                                job: 'open source/uiucprescon_speedwagon_workflows/master',
                                parameters: [
                                    booleanParam(name: 'RUN_CHECKS', value: true),
                                    booleanParam(name: 'USE_SONARQUBE', value: params.USE_SONARQUBE),
                                    booleanParam(name: 'TEST_RUN_TOX', value: true),
                                    booleanParam(name: 'BUILD_PACKAGES', value: true),
                                    booleanParam(name: 'PACKAGE_FOR_CHOCOLATEY', value: (params.INCLUDE_WINDOWS_X86_64 && isTriggeredByUser) || (isTriggeredByTimer && x86_64WindowsIsAvailable)),
                                    booleanParam(name: 'PACKAGE_STANDALONE_WINDOWS_INSTALLER', value: (params.INCLUDE_WINDOWS_X86_64 && isTriggeredByUser) || (isTriggeredByTimer && x86_64WindowsIsAvailable)),
                                    booleanParam(name: 'INCLUDE_LINUX-ARM64', value: params.INCLUDE_LINUX_ARM),
                                    booleanParam(name: 'INCLUDE_LINUX-X86_64', value: true),
                                    booleanParam(name: 'INCLUDE_MACOS-ARM64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (armMacIsAvailable && isTriggeredByTimer)),
                                    booleanParam(name: 'INCLUDE_MACOS-X86_64', value: (params.INCLUDE_MAC && isTriggeredByUser) || (x86_64MacIsAvailable && isTriggeredByTimer)),
                                    booleanParam(name: 'INCLUDE_WINDOWS-X86_64', value: (params.INCLUDE_WINDOWS_X86_64 && isTriggeredByUser) || (isTriggeredByTimer && x86_64WindowsIsAvailable)),
                                    booleanParam(name: 'PACKAGE_MAC_OS_STANDALONE_DMG', value: (params.INCLUDE_MAC && isTriggeredByUser) || (armMacIsAvailable && x86_64MacIsAvailable && isTriggeredByTimer)),
                                    booleanParam(name: 'DEPLOY_PYPI', value: false),
                                    booleanParam(name: 'DEPLOY_CHOCOLATEY', value: false),
                                    booleanParam(name: 'DEPLOY_DOCS', value: false)
                                ]
                        )
                    }
                }
            }
        }
    }
}
