#!groovy


node {
    stage 'checkout '
git credentialsId: 'c26a9e04-26cb-461b-8f36-589adfed3610', url: 'https://github.com/sasidharkanumuri/sasi'
    stage 'build '
    sh 'mvn clean install -f LoggingSample/pom.xml'
    stage 'sendEmail'
    mail bcc: '', body: "${env.BUILD_NUM}\n\n${env.BUILD_URL}", cc: '', from: '', replyTo: '', subject: 're: pipeline build', to: 'sasinrt@gmail.com'
stage 'parallel'
def branches = [:]
for (int i = 0; i < 4; i++) {
  def index = i //if we tried to use i below, it would equal 4 in each job execution.
  branches["branch${i}"] = {
    build job: 'deploy', parameters: [[$class: 'StringParameterValue', name: 'environment', value:
      'dev'], [$class: 'StringParameterValue', name:'numservers', value: "${index}"]]
    }
}
parallel branches

    stage 'qa approval'
    input 'proceed to qa'
}
