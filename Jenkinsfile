#!/usr/bin/env groovy

pipeline {
    agent none
    stages {
        stage("build") {
            parallel {
                'win32' : {
                    node {
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
                            deleteDir()
                        }
                    }
                },
                "win64" : {
                    node {
                        echo "win64"
                    }
                },
                "android" : {
                    node {
                        echo "android"
                    }
                }
            }
        }
        stage("test") {
            parallel {
                'unit' : {
                    node {
                        dir('unit') {
                            unstash 'result'
                            pwd
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
                    node {
                        dir('acceptance') {
                            unstash 'result-test'
                            pwd
                            sh 'ls'
                        }
                    }
                    post {
                        always {
                            deleteDir()
                        }
                    }
                },
            }
        }
    }
)
