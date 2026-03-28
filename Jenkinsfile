def deployApp(String environment, String port) {
    bat "set PM2_HOME=C:\\pm2_home && \"C:\\Users\\dzein\\AppData\\Roaming\\npm\\pm2.cmd\" delete greetings-app-${environment} & exit 0"
    bat """
        if exist C:\\app\\greetings-app-${environment} rmdir /s /q C:\\app\\greetings-app-${environment}
        mkdir C:\\app\\greetings-app-${environment}
        xcopy /E /I /Y python-greetings C:\\app\\greetings-app-${environment}
    """
    dir("C:\\app\\greetings-app-${environment}") {
        bat 'py -m venv venv'
        bat 'venv\\Scripts\\python -m pip install -r requirements.txt'
        bat "set PM2_HOME=C:\\pm2_home && \"C:\\Users\\dzein\\AppData\\Roaming\\npm\\pm2.cmd\" start app.py --name greetings-app-${environment} --interpreter C:\\app\\greetings-app-${environment}\\venv\\Scripts\\python.exe -- --port ${port}"
    }
}

def runTests(String environment) {
    dir('course-js-api-framework') {
        bat 'npm install'
        bat "npm run greetings greetings_${environment}"
    }
}

pipeline {
    agent any

    stages {
        stage('install-pip-deps') {
            steps {
                echo 'Installing all required pip dependencies...'
                checkout scm
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/mtararujs/python-greetings.git']],
                    extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'python-greetings']]
                ])
                dir('python-greetings') {
                    bat 'py -m venv venv'
                    bat 'venv\\Scripts\\python -m pip install -r requirements.txt'
                }
            }
        }

        stage('deploy-to-dev') {
            steps {
                echo 'Deploying application to DEV environment on port 7001...'
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/mtararujs/python-greetings.git']],
                    extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'python-greetings']]
                ])
                script { deployApp('dev', '7001') }
            }
        }

        stage('tests-on-dev') {
            steps {
                echo 'Running tests on DEV environment...'
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/mtararujs/course-js-api-framework.git']],
                    extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'course-js-api-framework']]
                ])
                script { runTests('dev') }
            }
        }

        stage('deploy-to-stg') {
            steps {
                echo 'Deploying application to STG environment on port 7002...'
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/mtararujs/python-greetings.git']],
                    extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'python-greetings']]
                ])
                script { deployApp('stg', '7002') }
            }
        }

        stage('tests-on-stg') {
            steps {
                echo 'Running tests on STG environment...'
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/mtararujs/course-js-api-framework.git']],
                    extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'course-js-api-framework']]
                ])
                script { runTests('stg') }
            }
        }

        stage('deploy-to-preprod') {
            steps {
                echo 'Deploying application to PREPROD environment on port 7003...'
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/mtararujs/python-greetings.git']],
                    extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'python-greetings']]
                ])
                script { deployApp('preprod', '7003') }
            }
        }

        stage('tests-on-preprod') {
            steps {
                echo 'Running tests on PREPROD environment...'
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/mtararujs/course-js-api-framework.git']],
                    extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'course-js-api-framework']]
                ])
                script { runTests('preprod') }
            }
        }

        stage('deploy-to-prod') {
            steps {
                echo 'Deploying application to PROD environment on port 7004...'
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/mtararujs/python-greetings.git']],
                    extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'python-greetings']]
                ])
                script { deployApp('prod', '7004') }
            }
        }

        stage('tests-on-prod') {
            steps {
                echo 'Running tests on PROD environment...'
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/mtararujs/course-js-api-framework.git']],
                    extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'course-js-api-framework']]
                ])
                script { runTests('prod') }
            }
        }
    }
}