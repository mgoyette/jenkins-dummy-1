#!/usr/bin/env groovy

pipeline {
    agent none
    stages {
        stage("build") {
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

                                echo 'win32-tests'
                                unstash 'result'
                                sh 'touch ex2.txt'
                                stash 'result-test'
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
        stage("test") {
            agent any
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
