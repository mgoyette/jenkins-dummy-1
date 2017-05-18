parallel (
    'win32' : {
        node {
            dir('build-win32') {
                try {
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
                finally {
                    deleteDir()
                }
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
            dir('unit') {
                try {
                    unstash 'result'
                    pwd
                    sh 'ls'
                }
                finally {
                    deleteDir()
                }
            }
        }
    },
    "acceptance-test" : {
        node {
            dir('acceptance') {
                try {
                    unstash 'result-test'
                    pwd
                    sh 'ls'
                }
                finally {
                    deleteDir()
                }
            }
        }
    }
)
