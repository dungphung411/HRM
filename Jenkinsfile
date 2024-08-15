pipeline {  
 agent any  
 environment {  
  dotnet = 'C:\\Program Files\\dotnet\\dotnet.exe'  
 }  
 stages {  
  stage('Checkout') {  
   steps {
       git branch: 'main', changelog: false, poll: false, url: 'https://github.com/dungphung411/HRM.git'
     }
  }

 stage('Build') {  
   steps {  
    bat 'dotnet build' 
    //bat 'dotnet build C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\HRMPipelines\\jenkins-demo\\HRM\\HRM.sln --configuration Release'  
   }  
  }  
  stage('Test') {  
   steps {  
    bat 'cd HRM.API'
    bat 'dotnet test  --logger:trx'  
   }  
  }
  
  stage("Release"){
      steps {
      bat 'dotnet build'
      bat 'dotnet publish -c Release  /p:WebPublishMethod=Package /p:PackageLocation=./bin/Zip/net8.0/publish/HRM.API.zip'
    }
  }
  
  stage('Deploy') {
    steps {
    // Stop IIS
    bat 'net stop "w3svc"'
    
    // Deploy package to IIS
    bat '"C:\\Program Files (x86)\\IIS\\Microsoft Web Deploy V3\\msdeploy.exe" -verb:sync -source:package="%WORKSPACE%\\HRM\\HRM.API\\bin\\Zip\\net8.0\\publish\\HRM.API.zip" -dest:auto -setParam:"IIS Web Application Name"="HRM.Web" -skip:objectName=filePath,absolutePath=".\\\\PackageTmp\\\\Web.config$" -enableRule:DoNotDelete -allowUntrusted=true'
    
    // Start IIS again
    bat 'net start "w3svc"'
    }
 }

 }  
} 