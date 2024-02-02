//Angent should have Java,Terrafrom,Aws cli,node js
//Stage-01 Get the version of the aplication.While doing the developments the application version will change (package.json file line no 3)
//Stage-02 Install Dependencies 
//Stage-03 Build i.e Zip all files(16_Jenkins_Catalogues ) and create artifact
//Stage-04 

pipeline {
    agent {
        node {
            label 'Agent' //Name of the Agent label 
        }
    }
    environment { 
        packageVersion = ''
        nexusURL = '172.31.5.95:8081' //Mention your Nexus Url
    }
    options {
        timeout(time: 1, unit: 'HOURS')
        disableConcurrentBuilds()  //It won't allow us to run two builds at a time.
    }
    // build
    stages {
        stage('Get the version') { 
            steps {
                script { //Groovy scripting. def command is used declare a variable in pipeline.
                    def packageJson = readJSON file: 'package.json' //Now we will get all data from the package.json file and stores in packageJson by using Pipeline Utility Steps plugin.
                    packageVersion = packageJson.version //Now from that file we can take the version id. 
                    echo "application version: $packageVersion" //Calling from line no 12
                }
            }
        }
        stage('Install dependencies') {
            steps {     //Shell commands in pipeline
                sh """   
                    npm install  
                """
            }
        }
        stage('Build') {
            steps {   // zip is a linux command to zip the files -x to exclude the files while zipping, -q to hide the running log, it is unnecessary and waste of memory.
                sh """
                    ls -la
                    zip -q -r catalogue.zip ./* -x ".git" -x "*.zip" 
                    ls -ltr
                """
            }
        }
    //     stage('Publish Artifact') {
    //         steps {
    //              nexusArtifactUploader(
    //                 nexusVersion: 'nexus3',
    //                 protocol: 'http',
    //                 nexusUrl: "${nexusURL}",
    //                 groupId: 'com.roboshop',
    //                 version: "${packageVersion}",
    //                 repository: 'catalogue',
    //                 credentialsId: 'nexus-auth',
    //                 artifacts: [
    //                     [artifactId: 'catalogue',
    //                     classifier: '',
    //                     file: 'catalogue.zip',
    //                     type: 'zip']
    //                 ]
    //             )
    //         }
    //     }
    //     stage('Deploy') {
    //         steps {
    //             script {
    //                     def params = [
    //                         string(name: 'version', value: "$packageVersion"),
    //                         string(name: 'environment', value: "dev")
    //                     ]
    //                     build job: "catalogue-deploy", wait: true, parameters: params
    //                 }
    //         }
    //     }
     }
    // post build
    post { 
        always { 
            echo 'I will always say Hello again!'
            deleteDir() //Once we create the zip file and store it nexus we have to delete the workspace folder.If not we face issue while running second time.
        }
        failure { 
            echo 'this runs when pipeline is failed, used generally to send some alerts'
        }
        success{
            echo 'I will say Hello when pipeline is success'
        }
    }
}