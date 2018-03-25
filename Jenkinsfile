node ('spark') {
     deleteDir()
try {
stage('Checkout') {
     checkout scm
     sh "cp input.txt /tmp/"
     sh "rm -rf /tmp/output"
}
stage('Run Python Spark') {
     sh "cd /opt/spark;./bin/spark-submit $WORKSPACE/wordcount.py"
}
stage('Result') {
     sh " cat /tmp/output/* "
}
// notify through email
 emailext {
       subject: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
       body: """<p>SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
         <p>Check console output at "<a href="${env.BUILD_URL}">${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>"</p>""",
       recipientProviders: [[$class: 'DevelopersRecipientProvider']]
      }
catch (e) {
    currentBuild.result = "FAILED"
    echo "{e.getClass().getName()} - ${e.getMessage()}"
    emailext {
       subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
       body: """<p>FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
         <p>Check console output at "<a href="${env.BUILD_URL}">${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>"</p>""",
       recipientProviders: [[$class: 'DevelopersRecipientProvider']]
       }
    throw e
   }
}
