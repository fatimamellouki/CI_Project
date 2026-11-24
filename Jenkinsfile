pipeline {
    agent any

    stages {

        stage('Clone') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/fatimamellouki/CI_Project.git'
            }
        }

        stage('Verification Python') {
            steps {
                bat 'python --version'
                bat 'python -m pip --version'
            }
        }

        stage('Installation des dependances') {
            steps {
                bat 'python -m pip install -r requirements.txt'
            }
        }

        stage('Tests unitaires') {
            steps {
                bat 'python -m pytest'
            }
        }
        stage('Analyse qualite code (Pylint)') {
            steps {
                bat 'python -m pylint calculator.py'
            }
        }
        stage('Creation de l artefact') {
            steps {
                bat '''
        if exist Release rmdir /S /Q Release
        if exist Release.zip del /Q Release.zip

        mkdir Release

        copy calculator.py Release
        copy test_calculator.py Release
        copy requirements.txt Release

        powershell Compress-Archive -Path Release\\* -DestinationPath Release.zip -Force
        '''
    }
}
    }

    post {
        always {
            echo 'Pipeline termine'
        }

        success {
            echo 'BUILD SUCCESS'
        }

        failure {
            echo 'BUILD FAILED'
        }
    }
}