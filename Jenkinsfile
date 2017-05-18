parallel (
    'win32' : {
        node {
            stage('build') {
                echo 'win32'
                sh 'touch ex.txt'
                stash 'result'
            }
            stage('build-tests') {
                echo 'win32-tests'
                unstash 'result'
                sh 'touch ex2.txt'
                stash 'result-test'
            }
        }
    },
    "win64" : {
        node {
            stage('build') {
                echo "win64"
            }
        }
    },
    "android" : {
        node {
            stage('build') {
                echo "android"
            }
        }
    }
)
parallel (
    "unit-test" : {
        node {
            unstash 'result'
            sh 'ls'
        }
    },
    "acceptance-test" : {
        node { 
            unstash 'result-test'
            sh 'ls'
        }
    }
)
