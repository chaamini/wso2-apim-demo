pipeline {
     
   agent any
  
   environment{
      INT_FILE = fileExists "master/Interceptors"
      LIB_FILE = fileExists "master/libs"
      APINAME = "master"
      REPO="chaamini/wso2-apim-demo"
   }
   
   stages {
      stage('Preparation') {   
         steps{
               git branch: "$APINAME",
               url: "https://github.com/${REPO}.git",
               credentialsId: 'github-chaamini'
         }
      } 
     
      stage('Deploying API to Kubernetes') {
         steps {
            echo "[INFO] - Deploying to Kubernetes - Begin"
            sh 'apictl set --mode k8s'
            sh 'echo running the build on top of the commit hash: $GIT_COMMIT'
            sh '/usr/bin/python /Users/chaamini/MyWork/Scripts/deployment-api.py createapi'
            echo "[INFO] - Deploying to Kubernetes - End"
         }
      }

      stage('Publishing API to Portal') {
         steps {
            echo "[INFO] - Publishing API to Portal - Begin"
            sh '/usr/bin/python /Users/chaamini/MyWork/Scripts/deployment-api.py tenant'
            echo "[INFO] - Publishing API to Portal - End"
         }
      }
   }
   
   post {
      always { 
         cleanWs()
      }
    }
}
