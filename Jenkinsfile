pipeline {
    agent {
        label 'bilostotskyi_node'
    }
    parameters {
        choice choices: ['DEVELOP', 'RELEASE'], description: '', name: 'RELEASE'
        string defaultValue: '0.0.1', description: '', name: 'RELEASE_VER', trim: false
    }

    tools {
        nodejs 'Node12'
    }
    stages {
        stage('Archive') {
            when {
                expression {
                    params.RELEASE == 'RELEASE'
                }
            }
            steps {
                archiveArtifacts '*.tgz'
            }
        }
            steps {
                echo "BLABLA"
            }
        }		
    }
}
