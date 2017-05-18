parallel {
    "win32" : {
        node('master') {
            echo "win32"
            sh "touch ex.txt"
            stash 'result'
        }
    }
    "win64" : {
        node('master') {
            echo "win64"
        }
    }
    "android" : {
        node('master') {
            echo "android"
        }
    }
}
parallel {
    "unit-test" : {
        node("master") {
            unstash 'result'
            sh 'ls'
        }
    }
    "acceptance-test" : {
        node("master") { 
            unstash 'result'
            sh 'ls'
        }
    }
}
