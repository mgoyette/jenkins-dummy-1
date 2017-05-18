node('master') {
    stage('Setup') {
        echo 'Preparing multiple builds'
    }
}
parallel (
    "web" : {
        node('master') {
            stage('Build') {
                echo 'Build web application'
            }
            stage('Test') {
                echo 'Selenium Tests'
            }
        }
    },
    "embedded" : {
        node('master') {
            stage('Build') {
                echo 'Build embedded solution'
            }
        }
        stage('Test') {
            parallel (
                "test-lowend" : {
                    node('master') {
                        echo 'Test lowend embedded'
                    }
                }, 
                "test-highend" : {
                    node('master') {
                        echo 'Build highend embedded'
                    }
                }
            )
        }
    }
)
