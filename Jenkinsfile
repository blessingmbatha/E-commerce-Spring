
pipeline {
  agent any
      tools{
        jdk 'jdk17'
        maven 'maven3'
      }

  stages{ 
    stage('Git Checkout'){
        steps{
               git changelog: false, poll: false, url: 'https://github.com/blessingmbatha/E-commerce-Spring.git'
      }
    }

    stage('Code Compile'){
        steps{
              sh "mvn compile"
      }
    }

    
   }
}
