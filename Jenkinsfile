pipeline{
  agent any
  tools{
    maven 'maven-3.9.6'
  }
  parameters{
     choice(name:'VERSION',choices:['1.1.0','1.2.0','1.3.0'],description:' ')
     booleanParam(name:'executeTests',defaultValue:true,description:' ')
  }
  stages{
    
    stage("build"){
      steps{
        echo 'building the application...'
      }
    }

    stage("build jar"){
      steps{
        echo "building the application..."
        sh 'mvn package'
      }
    }

    stage("test"){
      when{
        expression{
          params.executeTests
        }
      }
      steps{
        echo 'testing the application...'
      }
    }

    stage("build image"){
      steps{
        echo "building the docker image"
        withCredentials([usernamePassword(credentialsId:'docker-hub',passwordVariable:'PASS',usernameVariable:'USER')])
        sh 'docker build -t lavish11/my-repo:2.0 .'
        sh "echo $PASS | docker login -u $USER --password-stdin"
        sh 'docker push lavish11/my-repo:2.0'
      }
    }

    stage("deploy"){
      input{
        message "select environment to deploy to"
        ok "Done"
        parameters{
          choice(name:'ENV',choices:['Dev','Staging','Prod'],description:' ')
        }
      }
      steps{
        echo 'deploying the application...'
        echo "deploying version ${params.VERSION}"
        echo "deploying application in ${ENV} server"
      }
    }
    
  }
}
