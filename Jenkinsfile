pipeline {
agent any

stages {
/**Insurance-Backend Pipeline Job Build and Test stages **/
stage('SCM Checkout') {
steps {
git url:'https://github.com/arunimaU/INGFavBank.git'
}
}
stage('Build') {
steps {
         sh"/opt/apache-maven-3.6.3/bin/mvn clean package -Dmaven.test.skip=true "
}
}

          

stage('Deploy') {
steps {
                     sh"/opt/apache-maven-3.6.3/bin/mvn clean deploy -Dmaven.test.skip=true "
}
}
stage('Release') {
steps {
                    sh"export JENKINS_NODE_COOKIE=dontKillMe; nohup java -jar $WORKSPACE/target/*.jar &"
}
}

 stage('SIT Approval'){

 steps{

 slackSend baseUrl: 'https://hooks.slack.com/services/', channel: 'slackjenkins', color: 'bad', message: 'war file created', tokenCredentialId: 'arunimaslack', username: 'arunimauniyal'

 }

 }

        stage('Please Provide Approval for SIT Deployment ?'){

          steps{

            script{

                def userInput

  try {

    userInput = input(

        id: 'Proceed1', message: 'SIT Deployment Approval', parameters: [

        [$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please Confirm you agree with this']

        ])

} catch(err) {

    def user = err.getCauses()[0].getUser()

    userInput = false

    echo "Aborted by: [${user}]"

}

          }

        }

        }

          stage('Deployment-SIT'){

            steps{

                sshPublisher(publishers: [sshPublisherDesc(configName: 'server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook  backend_input1.yml', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])

            }

          }





}
    
}

