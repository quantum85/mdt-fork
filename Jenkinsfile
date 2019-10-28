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
		stage('Nexus') {
            steps {
                       shell('''
            curl -v -F r=raw-demo-hosted -F hasPom=false -F e=tgz -F g=site-archive -F a=site-archive -F v=1.0 -F p=tgz -F file=@site-archive.tgz -u jenkins-demo:$jenkins-demo https://master.jenkins-practice.tk:9443
              '''.stripMargin()
            )
            }
        }		
    }
}
