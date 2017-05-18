#!/usr/bin/env groovy

pipeline {
    agent none
    stages {
        stage("build") {
            steps {
                parallel (
                    'win32' : {
                        node('master') {
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
                        post {
                            always {
                                dir('win32') {
                                    sh 'ls'
                                }
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
                        post {
                            always {
                                dir('unit') {
                                    sh 'ls'
                                }
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
                        post {
                            always {
                                dir('acceptance') {
                                    sh 'ls'
                                }
                            }
                        }
                    }
                )
            }
        }
    }
}
