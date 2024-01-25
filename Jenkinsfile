pipeline {
    agent { label 'mac-mini-slave' }

    parameters { 
        choice(
            name: 'buildVariant', 
            choices: ['Debug_Scan_Only', 'Debug_TestFlight','Release_AppStore_TestFlight'],
            description: 'The variants to build'
        )
    }

    environment {
        LC_ALL = 'en_US.UTF-8'
        APP_NAME = 'SimpleCart'
        BUILD_NAME = 'SimpleCart'
        APP_TARGET = 'SimpleCart'
        APP_PROJECT = 'SimpleCart/SimpleCart.xcodeproj'
        APP_WORKSPACE = 'SimpleCart.xcworkspace'
        APP_TEST_SCHEME = 'SimpleCart/SimpleCartTest'

        // Define the paths and environment variables
        HOME_PATH = sh(script: 'echo $HOME', returnStdout: true).trim()
        XCODE_PATH = '/Applications/Xcode.app'
        FASTLANE_PATH = sh(script: 'echo $HOME/.fastlane', returnStdout: true).trim()
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install necessary dependencies using Homebrew
                script {
                    sh 'brew install fastlane'
                }
            }
        }

        stage('Build and Test') {
            steps {
                // Build and test your iOS project
                script {
                    sh "xcodebuild clean build -workspace SimpleCart.xcworkspace -scheme SimpleCartTest"
                }
            }
        }

        stage('Deploy with Fastlane') {
            steps {
                // Use Fastlane to deploy the application
                script {
                    sh "fastlane ios beta"
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                // Archive build artifacts
                archiveArtifacts artifacts: '**/build/*.ipa', fingerprint: true
            }
        }
    }

    post {
        always {
            // Clean up and perform any post-build actions
            script {
                sh "rm -rf $HOME_PATH/Library/Developer/Xcode/DerivedData"
                sh "killall Xcode || true"
            }
        }
    }
}