pipeline {
    agent any

    stages {
        stage('build') {
            steps {
                echo 'Building..'
            }
        }
        stage("Testing") {
            steps {
                parallel (
                    "Unit" : {
                        //do some stuff
                    },
                    "Acceptance" : {
                        // Do some other stuff in parallel
                    }
                )
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
