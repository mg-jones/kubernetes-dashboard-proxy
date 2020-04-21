node('Ubuntu') {
    String jenkins_wksp = pwd()
    def config
    def tmpDir
    def chartDir

    // Checkout latest changes
    stage('Checkout') {
        scmInfo = checkout scm
    }
    // Parse chart config and setup build directory
    // Helm requires the directory name to match the chart name
    stage('Setup') {
        config = readYaml( file: 'Chart.yaml' )
        tmpDir = '/var/tmp/' + scmInfo.GIT_COMMIT
        chartDir = tmpDir + '/' + config.name
        dir(chartDir) {
            sh "cp -R ${jenkins_wksp}/* ."
        }
    }
    // Helm linting
    stage('Test') {
        dir(chartDir) {
            sh "helm -f values-stage.yaml lint ."
            sh "helm -f values-stage.yaml template test ."
        }
    }
    // Cleanup work directories
    stage('Cleanup') {
        dir(tmpDir) {
            deleteDir()
        }
    }
}
