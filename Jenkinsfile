pipeline {
    agent any

    parameters {
        string(defaultValue: 'pypi_local', description: 'Name of PyPi Target', name: 'PyPiDistributionTargetName')
    }

    stages {
        stage ('Clean') {
            steps {
                deleteDir()
            }
        }
        stage('Checkout GIT'){
            steps {
                checkout scm

            }
        }
        stage('Build package'){
            steps {
                sh '''
                sudo rm -rf .venv
                virtualenv .venv

                .venv/bin/pip install nose
                .venv/bin/python setup.py build
                .venv/bin/python setup.py install
                .venv/bin/python setup.py sdist
                '''
            }
        }
        stage('Run tests') {
            steps {
                sh '''
                for test in tests/*.py; do echo $test ; .venv/bin/python $test > /dev/null ; done
                '''
            }
        }
        stage('Publish'){
            steps {
                sh """
                .venv/bin/python setup.py sdist upload -r ${params.PyPiDistributionTargetName}
                """
            }
        }
    }

}

