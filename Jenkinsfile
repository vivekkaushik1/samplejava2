
def appName ='Upload_App'
def namePath ='/api/retest/11'
def deplyName = 'TEST'
def response =''
def dataFormat ='json'
def fileName ='deployable'
def target ='deployable'
def commit ='true'
def validate ='true'
def changesetNumber =''
def finalresponse =''
pipeline {
    agent any
    stages {
        stage('publish') {
            steps {
                echo "<<<<<<<<<Upload file is starting >>>>>>>>"
          script{
               //with changesetNumber and deployable
                response = snDevOpsConfigUpload(applicationName: "${appName}", deployableName: "${deplyName}",dataFormat: "${dataFormat}",fileName: "${fileName}",target:"${target}",namePath: "${namePath}",autoCommit:"false",autoValidate:"${validate}",changesetNumber:"${changesetNumber}")
                response = snDevOpsConfigUpload(applicationName: "${appName}", deployableName: "Prod",dataFormat: "${dataFormat}",fileName: "${fileName}",target:"${target}",namePath: "${namePath}",autoCommit:"${commit}",autoValidate:"${validate}",changesetNumber:"${response}")
                //without deployableName
                // response = snDevOpsConfigUpload(applicationName: "${appName}",dataFormat: "${dataFormat}",fileName: "${fileName}",target:"${target}",namePath: "${namePath}",autoCommit:"${commit}",autoValidate:"${validate}",changesetNumber:"${changesetNumber}")
                //without changesetNumber
                //  response = snDevOpsConfigUpload(applicationName: "${appName}", deployableName: "${deplyName}",dataFormat: "${dataFormat}",fileName: "${fileName}",target:"${target}",namePath: "${namePath}",autoCommit:"${commit}",autoValidate:"${validate}")
                //without changesetNumber and deployableName
                // response = snDevOpsConfigUpload(applicationName: "${appName}",dataFormat: "${dataFormat}",fileName: "${fileName}",target:"${target}",namePath: "${namePath}",autoCommit:"${commit}",autoValidate:"${validate}")
                // finalresponse = snDevOpsConfigUpload(applicationName: "Test_app_1",dataFormat: "${dataFormat}",fileName: "component4",target:"${target}",namePath: "${namePath}/upload2",autoCommit:"${commit}",autoValidate:"${validate}",changesetNumber:"${response}")

            }
                echo " RESPONSE FROM UPLOAD : ${response}"
                // echo " RESPONSE FROM FINAL UPLOAD: ${finalresponse}"
            }
        }
        stage('export'){
            steps{
                echo "export started."
                echo " ++++++++++++"
                // snDevOpsChange()
                echo "export finished successfully."
            }
        }
    }
}
