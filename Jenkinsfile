  
      def appName='Upload_App'
      def snapName=''
      def deployName = 'TEST'
      def exportFormat ='json'
      def configFilePath = "paymentService"
      def fileNamePrefix ='exported_file_'
      def fullFileName="${appName}-${deployName}-${currentBuild.number}.${exportFormat}"
      def changeSetId=""
      def snapshotName=""
      def exporterName ='exporterWithARGS_3' 
pipeline {
    agent any
    stages {
        stage('Clone repository') {               
             
            checkout scm    
      }     
      stage('Build image') {       

            snDevOpsStep()

            app = docker.build("santoshnrao/demo-training-studio")    
       }     
//       stage('Test image') {           
//             app.inside {            
             
//              sh 'echo "Tests passed"'        
//             }    
//         }     
//        stage('Push image') {
//                   sh 'ls -a'
//                   docker.withRegistry('https://registry.hub.docker.com', 'santoshnrao-dockerhub') {            
//                   app.push("${env.BUILD_NUMBER}")            
//                   app.push("latest")        
//               }    

//            }

      
      stage('Validate Configurtion file'){
            

            sh "echo validating configuration file ${configFilePath}.${exportFormat}"
            changeSetId = snDevOpsConfigUpload(applicationName:"${appName}",target:'component',namePath:'paymentservice.v1.1', fileName:"${configFilePath}", autoCommit:'true',autoValidate:'true',dataFormat:"${exportFormat}")

            echo "validation result $changeSetId"
      }

    stage("register change set to pipeline"){
        echo "Change set registration for ${changeSetId}"
        changeSetRegResult = snDevOpsConfigRegisterChangeSet(changesetId:"${changeSetId}")
        echo "change set registration set result ${changeSetRegResult}"
    }

    stage("Get snapshots created"){
          
        echo "Triggering Get snapshots for applicationName:${appName},deployableName:${deployName},changeSetId:${changeSetId}"

        changeSetResults = snDevOpsConfigGetSnapshots(applicationName:"${appName}",deployableName:"${deployName}",changeSetId:"${changeSetId}")
        echo "ChangeSet Result : ${changeSetResults}"
        
        def changeSetResultsObject = readJSON text: changeSetResults
//         def changeSetResultsObject = jsonSlurper.parseText("${changeSetResults}")
        
          changeSetResultsObject.each {

                if(it.validation == "passed"){
                      echo "validation passed for snapshot : ${it.name}"
                      snapshotName = it.name
                }else{
                      echo "Snapshot failed to get validated : ${it.name}" ;
                      assert it.validation == "passed"
                }
            
          }
          if (!snapshotName?.trim()){
                error "No snapshot found to proceed" ;
          }
          echo "Snapshot Name : ${snapshotName} "                
          
    }

    stage('Publish the snapshot'){
        echo "Step to publish snapshot applicationName:${appName},deployableName:${deployName} snapshotName:${snapshotName}"
        publishSnapshotResults = snDevOpsConfigPublish(applicationName:"${appName}",deployableName:"${deployName}",snapshotName: "${snapshotName}")
        echo " Publish result for applicationName:${appName},deployableName:${deployName} snapshotName:${snapshotName} is ${publishSnapshotResults} "
    }

        stage('Deploy to the System'){
                echo "Devops Change trigger change request"
                snDevOpsChange()
              
        }

      stage('Download Snapshots from Service Now') {
            
            echo "Exporting for App: ${appName} Deployable; ${deployName} Exporter name ${exporterName} "
            echo "Configfile exporter file name ${fullFileName}"
            sh  'echo "<<<<<<<<<export file is starting >>>>>>>>"'
               response = snDevOpsConfigExport(applicationName: "${appName}", snapshotName: "${snapName}", deployableName: "${deployName}",exporterFormat: "${exportFormat}", fileName:"${fullFileName}",exporterName: "${exporterName}")
                echo " RESPONSE FROM EXPORT : ${response}"
        }
      
      stage("Deploying to PROD-US"){
            
                echo "Reading config from file name ${fullFileName}"
                echo " ++++++++++++ BEGIN OF File Content ***************"
                sh "cat ${fullFileName}"
                echo " ++++++++++++ END OF File content ***************"
                
                echo "deploy finished successfully."
            
      }
        
    }
}
