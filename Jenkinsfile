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
      /* stage('Releasing a new API version to GitHub') {
         steps {
            echo "Deploying new version of API"
            withCredentials([usernamePassword(credentialsId: 'github-chaamini', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                sh 'sh /Users/chaamini/MyWork/Scripts/git-release.sh $APINAME $USER $PASS $REPO'                        
            }
         }
      }  */   
     
      stage('Deploying API to Kubernetes') {
         steps {
            echo "[INFO] - Deploying to Kubernetes - Begin"
            sh 'apictl set --mode k8s'
            /*sh 'sh /Users/chaamini/MyWork/Scripts/deploying-api.sh $APINAME'*/
            sh 'echo running the build on top of the commit hash: $GIT_COMMIT'
            sh '/usr/bin/python /Users/chaamini/MyWork/Scripts/deployment-api.py createapi'
            echo "[INFO] - Deploying to Kubernetes - End"
         }
      }

      stage('Publishing API to Portal') {
         steps {
            echo "[INFO] - Publishing API to Portal - Begin"
/*            sh 'apictl set --mode k8s'
            sh 'apictl import-api -f $APINAME/ -e k8s -k'
*/
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
