pipeline{

    agent any

    stages{

       

         stage('Getting Code From GitHub')

      {

        steps {

              git 'https://github.com/arunimaU/INGFavBank.git'

              }

      }

        stage('Build'){

            steps{

             sh '/opt/maven/bin/mvn clean package'

            }

        }

 

        stage( 'email' ){

            steps{

                emailext (

 

  subject: "Waiting for your Approval! Job: '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",

 

  body: """<p>STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>

 

              <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",to: 'arunimauniyal@gmail.com'

 

 

 

)

            }

        }

                stage("Stage with input") {

 

    steps {

 

     

 

        script {

 

            def userInput = input(id: 'Proceed1', message: 'Promote build?', parameters: [[$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm you agree with this']])

 

            echo 'userInput: ' + userInput

 

 

 

            if(userInput == true) {

 

                // do action

 

            } else {

 

                // not do action

 

                echo "Action was aborted."

 

            }

 

 

 

        }  

 

    }

 

}

       

        stage('Copy jar and application.property file to Ansible Server'){

            steps{

             sshPublisher(publishers: [sshPublisherDesc(configName: 'LocalAnsible', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook backend_deploy.yml', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])

            }

        }

       

       

    }

}


