pipeline {
    agent any
    parameters {
        choice(
            name: 'BUILD_MODE',
            choices: ['With Docker', 'Without Docker'],
            description: 'Choose whether to build a Docker image'
        )
        choice(
            name: 'TARGET_ENV',
            choices: ['node-2-dev', 'node-2-qa'],
            description: 'Choose which environment to deploy to'
        )
        booleanParam(
            name: 'RUN_TESTS',
            defaultValue: true,
            description: 'Run test suite before deploying?'
        )
        string(
            name: 'RELEASE_NOTES',
            defaultValue: '',
            description: 'Optional release notes for this build'
        )
    }
    stages {
        stage('Info') {
            steps {
                echo "Branch: ${env.BRANCH_NAME}"
                echo "Build mode: ${params.BUILD_MODE}"
                echo "Target environment: ${params.TARGET_ENV}"
                echo "Run tests: ${params.RUN_TESTS}"
                echo "Release notes: ${params.RELEASE_NOTES}"
            }
        }
        stage('Test') {
            when {
                expression { return params.RUN_TESTS == true }
            }
            steps {
                echo "Running tests..."
                sh 'python3 check.py'
            }
        }
        stage('Build') {
            steps {
                script {
                    if (params.BUILD_MODE == 'With Docker') {
                        echo "Would build Docker image here"
                    } else {
                        echo "Skipping Docker — plain build only"
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    echo "Deploying to: ${params.TARGET_ENV}"
                }
            }
        }
    }
}