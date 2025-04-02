pipeline {
    agent any
    triggers{ pollSCM('*/1 * * * *') }
    

    stages {
        stage('install-pip-deps') {
            steps {
                script{
                    install()
                }
            }
        }
        stage('deploy-to-dev') {
            steps {
                script{
                    deploy("dev", 7001)
                }
            }
        }
        stage('tests-on-dev') {
            steps {
                script{
                    test("dev")
                }
            }
        }

        //Fake staging environment
        stage('deploy-to-staging') {
            steps {
                script{
                    deploy("staging", 7002)
                }
            }
        }
        stage('tests-on-staging') {
            steps {
                script{
                    test("staging")
                }
            }
        }

        //Fake preproduction environment
        stage('deploy-to-preprod') {
            steps {
                script{
                    deploy("preprod", 7003)
                }
            }
        }
        stage('tests-on-preprod') {
            steps {
                script{
                    test("preprod")
                }
            }
        }

        //Fake production environment
        stage('deploy-to-prod') {
            steps {
                script{
                    deploy("prod", 7004)
                }
            }
        }
        stage('tests-on-prod') {
            steps {
                script{
                    test("prod")
                }
            }
        }
    }
}

def install(){
    echo "Installing pip dependencies"
    git branch: 'main', poll: false, url: 'https://github.com/mtararujs/python-greetings.git'
    sh "ls"
    sh "python3 -m venv venv"
    sh "source venv/bin/activate"
    sh "venv/bin/pip install -r requirements.txt"
}

def deploy(String env, int port){
    echo "Deployment to ${env} has started"
    git branch: 'main', poll: false, url: 'https://github.com/mtararujs/python-greetings.git'
    sh "pm2 delete \"greetings-app-${env}\" || true"
    sh "pm2 start app.py --interpreter=/venv/bin/python --name \"greetings-app-${env}\" -- --port ${port}"
}

def test(String env){
    echo "Testing ${test_set} on ${env} has started"
    git branch: 'main', poll: false, url: 'https://github.com/mtararujs/course-js-api-framework.git'
    sh "npm install"
    sh "npm run greetings greetings_${env}"
}