#!/usr/bin/env groovy

pipeline {
    agent none
    stages {
        stage("build") {
            steps {
                parallel (
                    'win32' : {
                        node any {
                            steps {
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
                        }
                        post {
                            always {
                                deleteDir()
                            }
                        }
                    },
                    "win64" : {
                        node any {
                            echo "win64"
                        }
                    },
                    "android" : {
                        node any {
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
                        node any {
                            dir('unit') {
                                unstash 'result'
                                pwd()
                                sh 'ls'
                            }
                        }
                        post {
                            always {
                                deleteDir()
                            }
                        }
                    },
                    'acceptance' : {
                        node any {
                            dir('acceptance') {
                                unstash 'result-test'
                                pwd()
                                sh 'ls'
                            }
                        }
                        post {
                            always {
                                deleteDir()
                            }
                        }
                    }
                )
            }
        }
    }
}
