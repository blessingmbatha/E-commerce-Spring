
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

    stage('Run Code Test'){
        steps{
              sh "mvn test"
      }
    }

    stage('Sonarqube Analysis') {
        steps {
             withSonarQubeEnv('sonar-sever') {
             withMaven(maven:'maven3') {
             sh 'mvn clean package sonar:sonar'
             }
         }
      }
    }      
    
    stage('OWASP Dependency Check'){
        steps{
              dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'DC'
              dependencyCheckPublisher pattern: '**/Dependency-check-report.xml '
      }
    }
    
    stage('Maven Build'){
        steps{
              sh "mvn clean compile"
      }
    } 

    stage('Docker Build & Push') {
            steps {
                   script {
                       withDockerRegistry(credentialsId: 'docker-token', toolName: 'docker') {
                            sh "docker build -t project2 ."
                            sh "docker tag webapp nkosenhlembatha/project2:latest"
                            sh "docker push nkosenhlembatha/project2:latest "
                        }
                   } 
            }
      }

    stage('Trivy Scan Docker Image') {
           steps {
               script {
	                sh ("docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image nkosenhlembatha/project2:latest --no-progress --scanners vuln  --exit-code 0 --severity HIGH,CRITICAL --format table")
               }
           }
       }
   }
}
