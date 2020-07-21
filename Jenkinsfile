pipeline {
   agent any

   stages {
      stage('Build') {
         steps {
            // Run dotnet build to build the process.
           script{ datas = readYaml (file: 'template.yml') }
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
           sh "dotnet test ${datas.TEST_PARAMS}"
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
        environment {
            scannerHome = tool 'Sonar-dotnet'
        }
         steps {
            sh "echo '******Publishing project********'"

            // Run dotnet build to build the process.
            sh "dotnet publish"
            dir("${env.WORKSPACE}/${DIR_URL}"){
              withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws-credentials', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                sh "tar -czvf netcoreapp3.1.tar.gz publish"
                sh "aws s3 cp netcoreapp3.1.tar.gz s3://${BUCKET_NAME}/"
                //sh "aws s3 cp . s3://${BUCKET_NAME}/ --recursive"
              }
            }  
           
           
            sh "${scannerHome}/sonar-scanner-2.8/bin/sonar-scanner /d:sonar.login=admin /d:sonar.password=admin"
          
            
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
