pipeline{
    agent any

    environment {
      CHROME_VERSION = '127.0.6533.73'
      CHROMEDRIVER_VERSION = '127.0.6533.72'
      CHROME_INSTALL_PATH = 'C:\\Program Files\\Google\\Chrome\\Application'
      CHROMEDRIVER_PATH = 'C:\\Users\\Mihail\\.cache\\selenium\\chromedriver\\win64\\133.0.6943.98'
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
