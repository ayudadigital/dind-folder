#!groovy

@Library('github.com/ayudadigital/jenkins-pipeline-library@v6.2.0') _

// Initialize global config
cfg = jplConfig('dind-folder', 'bash', '', [email: env.CI_NOTIFY_EMAIL_TARGETS])

pipeline {
    agent { label 'docker' }

    stages {
        stage ('Initialize') {
            steps  {
                jplStart(cfg)
            }
        }
        stage ('Make release') {
            when { branch 'release/new' }
            steps {
                jplMakeRelease(cfg, true)
                deleteDir()
            }
        }
    }

    post {
        always {
            jplPostBuild(cfg)
        }
    }

    options {
        timestamps()
        ansiColor('xterm')
        buildDiscarder(logRotator(artifactNumToKeepStr: '20',artifactDaysToKeepStr: '30'))
        disableConcurrentBuilds()
        timeout(time: 1, unit: 'DAYS')
    }
}
