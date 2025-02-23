pipeline {
    agent any

    environment {
        DOTNET_ROOT = "/usr/share/dotnet"
        PATH = "$DOTNET_ROOT:$PATH"
    }

    stages {
        stage('Setup .NET 8') {
            steps {
                sh '''
                    # Install .NET 8 if not present
                    if ! dotnet --list-sdks | grep -q "8.0"; then
                        echo "Installing .NET 8..."
                        wget https://dot.net/v1/dotnet-install.sh -O dotnet-install.sh
                        chmod +x dotnet-install.sh
                        ./dotnet-install.sh --channel 8.0
                        echo "Done installing .NET 8."
                    fi
                    
                    # Verify .NET 8 installation
                    dotnet --version
                '''
            }
        }

        stage('Restore Dependencies') {
            steps {
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build --no-restore'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'dotnet test --no-build --verbosity normal'
            }
        }
    }
}
