#!/usr/bin/env groovy

pipeline {
    agent none
    stages {
        stage("build libstack") {
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
        stage("build stacksystemtest") {
            agent any
            steps {
                unstash 'result'
                sh 'touch ex2.txt'
                stash 'result-test'
            }
        }
        stage("run network tests") {
            agent any
            steps {
                parallel (
                    'register' : { 
                        script {
                            try {
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
                                unstash 'none'
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
        stage("execute test") {
            agent any
            when {
                expression {
                    currentBuild.result == null || currentBuild.result == 'SUCCESS' 
                }
            }
            steps {
                parallel (
                    'unit' : {
                        node('master') {
                            deleteDir()
                            dir('unit') {
                                unstash 'result'
                                sh 'ls'
                            }
                        }
                    },
                    'acceptance' : {
                        node('master') {
                            deleteDir()
                            dir('acceptance') {
                                unstash 'result-test'
                                sh 'ls'
                            }
                        }
                    }
                )
            }
        }
    }
}
