import groovy.util.*;

pipeline {
    agent any

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

    options {
        skipDefaultCheckout(true)
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                checkout scm
            }
        }

        // stage('Install Dependencies') {
        //     steps {
        //         // Install necessary dependencies using Homebrew
        //         script {
        //             sh 'brew install fastlane'
        //         }
        //     }
        // }

        stage('Dot Files Check') {
            steps {
                script {
                    sh "if [ -e .gitignore ]; then echo '.gitignore found'; else echo 'no .gitignore found' && exit 1; fi"
                }
            }
        }

        stage('Building') {
            steps {
                script {
                    if (env.BUILD_VARIANT == 'Debug_Scan_Only') {
                        stage ('Scan - Build only') {
                            sh "xcodebuild -workspace ${env.APP_WORKSPACE} -scheme ${env.APP_SCHEME} -sdk iphoneos -configuration \"${env.APP_BUILD_CONFIG}\" CODE_SIGN_IDENTITY=\"\" CODE_SIGNING_REQUIRED=NO CODE_SIGN_ENTITLEMENTS=\"\" CODE_SIGNING_ALLOWED=\"NO\" build"
                        }
                    }
                }
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