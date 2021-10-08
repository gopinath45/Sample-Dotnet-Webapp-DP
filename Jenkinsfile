pipeline{
    agent{
        label 'win10'
    }

    stages{

        stage ('Clear WS'){
            steps{
                cleanWs()
            }
        }
        
        stage ('SCM Checkout'){
            steps{
                git 'https://github.com/gopinath45/Sample-Dotnet-Webapp-DP.git'
            }
        }

        stage ('Restore Nuget Packages'){
            steps{
                bat 'C:\\Tools\\Nuget\\nuget.exe restore "%WORKSPACE%\\WebApplication1\\WebApplication1.sln"'
            }
        }

        stage ('Build + Sonar Analysis'){
            steps{
                script{
                    def scannerHome = tool 'Scanner_MSBuild_5.3.1'
                    withSonarQubeEnv('Sonarqube') {
                    bat "${scannerHome}\\SonarScanner.MSBuild.exe begin /k:dotnet"
                    bat "MSBuild.exe WebApplication1\\WebApplication1.sln /p:DeployOnBuild=true"
                    bat "${scannerHome}\\SonarScanner.MSBuild.exe end"
                    }
                }
                
            }
        }

        stage ('Quality Gate'){
            steps{
                timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
                }
            }
        }
        
    }
}
