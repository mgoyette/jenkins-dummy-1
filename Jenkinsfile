#!/usr/bin/env groovy

pipeline {
    agent none
    stages {
        stage("build win32") {
            agent { node 'master' }
            steps {
                deleteDir()
                dir('win32') {
                    echo 'win32'
                    sh 'touch ex.txt'
                    stash 'result'
                }
            }
        }
        stage("build win32 tests") {
            agent { node 'master' }
            steps {
                dir('libtest') {
                    deleteDir()
                    unstash 'result'
                    sh 'ls'
                }
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
