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

        stage('Build') {

            parallel {

                stage('JS') {

                    steps {

                        sh label: 'minimize JS', script: """

                        cd ${WORKSPACE}/www/js

                        uglifyjs --timings init.js -o ../min/custom-min.js

                        """

                    }

                }

                stage('CSS') {

                    steps {

                        sh label: 'minimize CSS', script: """

                        cd ${WORKSPACE}/www/css

                        cleancss -d style.css > ../min/custom-min.css"""

                    }

                }

                stage('Prepare artifact') {

                    when {

                      branch 'master'

                    }

                    steps {

                        sh label: 'archive', script: """

                        cd ${WORKSPACE}/www

                        tar --exclude='./css' --exclude='./js' -c -z -f ../site-archive-${params.RELEASE}-${params.RELEASE_VER}-${BUILD_NUMBER}.tgz ."""

                    }

                }

            }

        }

        stage('Archive') {

            when {

                expression {

                    params.RELEASE == 'RELEASE'

                }

                branch 'master'

            }

            steps {

                archiveArtifacts '*.tgz'

            }
         //end stage
        }
        stage('Nexus') {

              steps {
                nexusArtifactUploader artifacts: [
               [artifactId: 'nexus-artifact-uploader', classifier: 'debug', file: 'site-archive-DEVELOP-0.0.1-4.tgz', type: 'gzip'] 
               credentialsId: 'student8-jenkins',
               groupId: 'student-8role', 
               nexusUrl: 'https://master.jenkins-practice.tk:9443/', 
               nexusVersion: 'nexus3', 
               protocol: 'http', 
               repository: 'student8-repo', 
               version: '3'
              }

        }		

    }

}
