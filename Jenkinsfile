#!/usr/bin/env groovy

pipeline {
    agent none
    stages {
        stage("libstack") {
            agent any
            steps {
                parallel (
                    'win32' : {
                        node('master') {
                            deleteDir()
                            dir('win32') {
                                echo 'win32'
                                sh 'touch ex.txt'
                                stash 'result'
                            }
                        }
                    },
                    "win64" : {
                        node('master') {
                            deleteDir()
                            echo "win64"
                        }
                    },
                    "android" : {
                        node('master') {
                            deleteDir()
                            echo "android"
                        }
                    }
                )
            }
        }
        stage("unit tests") {
            agent any
            steps {
                unstash 'result'
                echo 'running tests'
            }
        }
        stage("stacksystemtest") {
            agent any
            steps {
                unstash 'result'
                sh 'touch ex2.txt'
                stash 'result-test'
            }
        }
        stage("network tests") {
            agent any
            steps {
                parallel (
                    'register' : { 
                        script {
                            try {
                                unstash 'result-test'
                                echo 'register-test'
                            }
                            catch(all) {
                                echo 'an error occured, setting to UNSTABLE'
                                currentBuild.result = 'UNSTABLE'
                            }
                        }
                    },
                    'call' : {
                        script {
                            try {
                                unstash 'result-test'
                                echo 'call-test'
                            }
                            catch(all) {
                                echo 'an error occured, setting to UNSTABLE'
                                currentBuild.result = 'UNSTABLE'
                            } 
                        }
                    }
                )
            }
        }
        stage("acceptance tests") {
            agent any
            when {
                expression {
                    currentBuild.result == null || currentBuild.result == 'SUCCESS' 
                }
            }
            steps {
                deleteDir()
                dir('acceptance') {
                    unstash 'result-test'
                    script {
                        tests = ['1':{echo '1'}, '2':{echo '2'}, '3':{echo '3'}, '4':{echo '4'}]
                        parallel (tests)
                    }
                }
            }
        }
    }
}
