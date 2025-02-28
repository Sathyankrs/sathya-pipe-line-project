pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Sathyankrs/sathya-pipe-line-project.git'
            }
        }
        stage('Deploy') {
            steps {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'web_server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'sudo systemctl restart nginx', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/var/www/html/jn.tagtes.com/', remoteDirectorySDF: '', removePrefix: '', sourceFiles: '**/*')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
    }
    post {
        failure {
            echo 'pipeline has failed'
        }
        success {
            echo 'pipeline has succeeded'
        }
    }
}