pipeline {
  agent any
  environment {
    registry = "sanyambatra/demo-pipeline"
    registryCredential = 'docker-hub'
    dockerImage = ''
    HOME = '/tmp'
    
  }

stages {
    
stage('Checkout') {
    
steps {
      script {
    def customImage = docker.build("my-image:${env.BUILD_ID}","./CalcMvcWeb/")

    customImage.inside {
        checkout scm
    }
}
    

    
}
    
}

stage('Build and Test') {
  steps {
    script {
    def customImage = docker.build("my-image:${env.BUILD_ID}","./CalcMvcWeb/")

    customImage.inside {
        sh 'dotnet build aspnetapp.sln'
        sh 'dotnet test'
    }
}
}
}
  stage('Build image') {
    steps {
      script{
      dockerImage=docker.build registry + ":$BUILD_NUMBER"
      }
    }
  }
  stage('Push image') {
    steps {
      script {
        
      docker.withRegistry( '', registryCredential ) {
          
        dockerImage.push()
}
       
      }
    }
  }
  stage('Deploy') {
    steps {
      script {
            def customImage = docker.build("my-image:${env.BUILD_ID}","./CalcMvcWeb/")

    customImage.inside {
                withCredentials([azureServicePrincipal('azure_cred')]) {
                  sh 'az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID'
          
                  sh 'az group create --name sanyamdemogroup1 --location eastus'
                  //sh 'az acr create --name sanyamregistry --resource-group sanyamdemogroup --sku Basic --admin-enabled true'

                  
                  sh 'az appservice plan create -n sanyamdemoplan1 -g sanyamdemogroup1 --sku S1 --is-linux'
                  sh 'az webapp create -g sanyamdemogroup1 -p sanyamdemoplan1 -n sanyamdemoapp1 --deployment-container-image-name sanyambatra/demo-pipeline:$BUILD_NUMBER'
                  //sh 'az webapp delete --name sanyamdemoapp --resource-group sanyamdemogroup'
                  //sh 'az webapp create -g sanyamdemogroup -p sanyamdemoplan -n sanyamdemoapp --deployment-container-image-name sanyambatra/demo-pipeline:$BUILD_NUMBER'
                  sh 'az webapp config appsettings set --resource-group sanyamdemogroup1 --name sanyamdemoapp1 --settings WEBSITES_PORT=80'
               
        }
    }

      }
    }
  }
}
}
