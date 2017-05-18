#!/usr/bin/env groovy

pipeline {
    agent none
    stages {
        stage("build") {
            steps {
                parallel (
                    'win32' : {
                        agent('any') {
                            dir('win32') {
                                echo 'win32'
                                sh 'touch ex.txt'
                                stash 'result'

                                echo 'win32-tests'
                                unstash 'result'
                                sh 'touch ex2.txt'
                                stash 'result-test'
                            }
                        }
                    },
                    "win64" : {
                        node('master') {
                            echo "win64"
                        }
                    },
                    "android" : {
                        node('master') {
                            echo "android"
                        }
                    }
                )
            }
            post {
                always {
                    deleteDir()
                }
            }
        }
        stage("test") {
            steps {
                parallel (
                    'unit' : {
                        node('master') {
                            dir('unit') {
                                unstash 'result'
                                pwd()
                                sh 'ls'
                            }
                        }
                    },
                    'acceptance' : {
                        node('master') {
                            dir('acceptance') {
                                unstash 'result-test'
                                pwd()
                            }
                        }
                    }
                )
            }
            post {
                always {
                    deleteDir()
                }
            }
        }
    }
}
