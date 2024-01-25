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
    }
}