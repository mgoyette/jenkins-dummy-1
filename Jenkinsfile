pipeline {
    agent any

    stages {
        stage('build') {
            steps {
                echo 'Building..'
            }
        }
        stage("Testing") {
            parallel (
                "Unit" : {
                    echo "unit"
                    //do some stuff
                },
                "Acceptance" : {
                    echo "acceptance"
                    // Do some other stuff in parallel
                }
            )
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
