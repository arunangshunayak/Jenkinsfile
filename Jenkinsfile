pipeline {
   agent any
   stages {
       stage('Code Clone') {
           steps {
           sh "rm -rf spring3hibernate; git clone https://gitlab.com/arunangshu_ops/spring3hibernate.git"
               sh "cd spring3hibernate; mvn compile"
           }
       }
       stage('Code Stability') {
           steps {
               sh "cd spring3hibernate; mvn install"
           }
       }
       stage('Code Quality'){
           steps{
               sh "cd spring3hibernate; mvn findbugs:findbugs"
           }
       }
       stage('Code Coverage'){
        steps{
               sh "cd spring3hibernate; mvn cobertura:cobertura "
        }
        }
       }
    post {
       always {
           wrap([$class: 'BuildUser'])
           {
           emailext body: "${currentBuild.currentResult}: Job ${JOB_NAME} build ${BUILD_NUMBER}\n More info at: ${env.BUILD_URL}\n User: ${BUILD_USER}",
           recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']],
           subject: 'Test'
           slackSend (message: "SUCCESSFUL: Job ${env.JOB_NAME} [${env.BUILD_NUMBER}]\n More info at: (${env.BUILD_URL})\n User: ${BUILD_USER}")
           recordIssues enabledForFailure: true, tools: [checkStyle(pattern: '**/spring3hibernate/target/checkstyle-result.xml')]
           recordIssues enabledForFailure: true, tools: [findBugs(pattern: '**/spring3hibernate/target/findbugs.xml')]
//          step([$class: 'CoberturaPublisher', autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: '**/coverage.xml', failUnhealthy: false, failUnstable: false, maxNumberOfBuilds: 0, onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false])
           step([$class: 'CoberturaPublisher', autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: '**/coverage.xml', conditionalCoverageTargets: '70, 0, 0', failUnstable: false, lineCoverageTargets: '80, 0, 0', maxNumberOfBuilds: 0, methodCoverageTargets: '80, 0, 0', onlyStable: false, packageCoverageTargets: '80, 0, 0', sourceEncoding: 'ASCII', zoomCoverageChart: false])
           }
       }
       success {
           echo 'This will run only if successful'
       }
       failure {
           echo 'This will run only if failed'
       }
       unstable {
           echo 'This will run only if the run was marked as unstable'
       }
       changed {
           echo 'This will run only if the state of the Pipeline has changed'
           echo 'For example, if the Pipeline was previously failing but is now successful'
       }
    }
}


