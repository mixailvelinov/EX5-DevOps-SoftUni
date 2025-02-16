pipeline{
    agent any

    environment {
      CHROME_VERSION = '127.0.6533.73'
      CHROMEDRIVER_VERSION = '127.0.6533.72'
      CHROME_INSTALL_PATH = 'C:\\Program Files\\Google\\Chrome\\Application'
      CHROMEDRIVER_PATH = 'C:\\Users\\Mihail\\.cache\\selenium\\chromedriver\\win64\\133.0.6943.98'
      PATH = "${env.PATH};C:\\ProgramData\\chocolatey\\bin" 

    }

    stages{
        stage("Checkout the code"){
            steps {
                git(
                  branch: 'main',
                  url: 'https://github.com/mixailvelinov/EXC5-devops'
            )
            }
        }

        stage("Clean up Chocolatey") {
          steps {
            bat 'del /f /q "C:\\ProgramData\\chocolatey\\lib\\KB2919442\\.chocolateyPending" || echo No .chocolateyPending file found'
          }
        }

        stage("Set up .NET Core"){
            steps{
              bat '''
                echo Installing .NET SDK 6.0
                choco install dotnet-sdk
                '''
            }
        }

        stage("Uninstall current chrome"){
            steps{
                bat '''
                echo Uninstalling current Google Chrome
                choco uninstall googlechrome -y
                '''
            }
        }

        stage("Install specific version of Chrome"){
            steps{
                bat '''
                choco Installing Google Chrome version %CHROME_VERSION%
                choco install googlechrome --version=%CHROME_VERSION% -y --allow-downgrade --ignore checksums
                '''
            }
        }

        stage("Download and install ChromeDriver"){
            steps {
                bat '''
                  echo Downloading ChromeDriver version %CHROMEDRIVER_VERSION%
                  powershell -command "Invoke-WebRequest -Uri https://chromedriver.storage.googleapis.com/%CHROMEDRIVER_VERSION%/chromedriver_win32.zip -OutFile chromedriver.zip -UseBasicParsing"
                  powershell -command "Expand-Archive -Path chromedriver.zip -DestinationPath ."
                  powershell -command "Move-Item -Path .\\chromedriver.exe -Destination '%CHROME_INSTALL_PATH%\\chromedriver.exe' -Force"
                    '''
                  } 
        }

        stage("Restore Dependencies Stage"){
            steps{
                bat 'dotnet restore'
            }
        }

        stage("Build") {
          steps {
            bat 'dotnet build --configuration Release'
          }
        }

        stage("Run tests") {
          steps {
            bat 'dotnet test'
          }
        }
  }

  post {
    always {
      bat "echo Pipeline is successful!"
    }
  }

}
