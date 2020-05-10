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
       stage('Releasing a new API version to GitHub') {
         steps {
            echo "Deploying new version of API"
            withCredentials([usernamePassword(credentialsId: 'github-chaamini', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                sh 'sh /Users/chaamini/MyWork/Scripts/git-release.sh $APINAME $USER $PASS $REPO'                        
            }
         }
      }     
     
      stage('Deploying API to Kubernetes') {
         steps {
         /*   echo "Deploying to Kubernetes"
            sh 'apictl set --mode k8s'
            sh 'sh /Users/chaamini/MyWork/Scripts/deploying-api.sh $APINAME'*/
            sh 'echo $GIT_COMMIT'
         }
      }

      stage('Publishing API to Portal') {
         steps {
/*            echo "Publishing to Portal"
            sh 'apictl set --mode k8s'
            sh 'apictl import-api -f $APINAME/ -e k8s -k'
  */
         }
      }
   }
   
   post {
      always { 
         cleanWs()
      }
    }
}
