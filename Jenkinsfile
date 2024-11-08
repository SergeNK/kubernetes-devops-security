pipeline {
  agent any

  stages {
      stage('Build Artifact') {
          steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' //so that they can be downloaded later
              echo "Poll jenkins testing"
          }
      }

      stage('Unit tests')  {
          steps {
              sh "mvn test"
          }
          post {
              always {
                  junit 'target/surefire-reports/*.xml'
                  jacoco execPattern: 'target/jacoco.exec'
              }
          }
      }

      stage('Docker Build and Push') {
        steps {
            withDockerRegistry([credentialsId: "docker-hub", url:""]) {
                sh 'printenv'
                sh 'who'
                sh 'docker build -t snj01/numeric-app:""$GIT_COMMIT"" .'
                sh 'docker push snj01/numeric-app:""$GIT_COMMIT""'
            }
        }
      }
  }
}
