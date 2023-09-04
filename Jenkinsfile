pipeline {
    agent any
    parameters {
        choice(choices: 'staging\nproduction', description: 'Which environment?', name: 'ENVIRONMENT')
    }
    environment {
        GIT_TAG = sh(returnStdout: true, script: 'git describe --always').trim()
    }
    stages {
        stage("Checkout") {
            steps {
                checkout scm
	    }
        }
        stage("Build") {
            agent {
                docker { image 'raptor1702:test' }
            }
            steps {
                sh 'docker build test'
                
	    }
        }        
        stage("Test") {
            steps {
                sh('chmod 777 run_all_tests.sh ')
                sh('./run_all_tests.sh')
	    }
        }
        stage("Deploy to Staging") {
            when {
                buildingTag()
                environment name: 'ENVIRONMENT', value: 'staging'
            }
            steps {
                sh('chmod 777 build_and_publish.sh')
                sh('./build_and_publish.sh')
	    }
        }
        stage("Deploy to Production") {
    when {
        buildingTag()
        environment name: 'ENVIRONMENT', value: 'production'
    }
    steps {
        sh('chmod 777 Deploytoprod.sh')
        sh('./Deploytoprod.sh')
    }
}
    }
}
