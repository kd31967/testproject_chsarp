pipeline{
    agent any
    
    stages{
        stage ('Clean WorkSpace Directory'){
            steps {
                cleanWs()
            }
        }
        stage ('Git CheckOut'){
            steps {
                //git branch: 'feature/newversion', url: 'https://github.com/rahulrathore44/RestSharpFramework.git' 
				git 'https://github.com/kd31967/testproject_chsarp.git'
				
            }
            
        }
        stage ('Restore Packages'){
            steps {
                bat 'H:\\DOTNET\\dotnetall\\NuGet.exe restore WebServiceAutomation.sln'
            }
        }
        stage ('Build'){
            steps {
                
                bat "\"${tool 'MSBUILD_14'}\" -verbosity:detailed WebServiceAutomation.sln /p:Configuration=Release /p:Platform=\"Any CPU\""
            }
        }
        stage ('Deploy'){
            steps {
                bat 'echo Deploying the application..'
            }
        }
        stage ('Run the Test') {
            steps {
                bat "\"${tool 'VSTest'}\" WebServiceAutomation/bin/Release/WebServiceAutomation.dll RestSharpAutomation/bin/Release/RestSharpAutomation.dll MsTestProject/bin/Release/MsTestProject.dll /InIsolation /Logger:html"
            }
        }
    }
    
    post {
        always{
            archiveArtifacts artifacts: 'TestResults/**/*.html', fingerprint: true
            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'TestResults', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: ''])
        }
    }
}