pipeline {
   agent any

   stages {
      stage('Build') {
         steps {
            // Run dotnet build to build the process.
            sh "dotnet build"
         }

         post {
            success {
               sh "echo 'good was built successfully'"
            }
         }
      }
      
       stage('Test') {
         steps {
            sh "echo '******Getting some Testing done********'"
            sh "cd "
            // Run dotnet build to build the process.
            sh "dotnet test $TEST_PARAMS"
         }

         post {
            success {
               sh "echo 'Tests were performed successfully'"
            }
            unstable {
               sh "echo 'Something went wrong with the testing'"
            }
         }
      }
      
      stage('Publish') {
         steps {
            sh "echo '******Publishing project********'"

            // Run dotnet build to build the process.
            sh "dotnet publish"
         }

         post {
            success {
               sh "echo 'Publishing was performed successfully'"
            }
            unsuccessful {
               sh "echo 'Something went wrong with the publishing.'"
            }
         }
      }
      
   }
}
