pipeline {
          agent { label "maven" }
          stages {
            stage("Clone Source") {
              steps {
                checkout([$class: 'GitSCM',
                            branches: [[name: '*/master']],
                            extensions: [
                              [$class: 'RelativeTargetDirectory', relativeTargetDir: 'mavenapp']
                            ],
                            userRemoteConfigs: [[url: 'https://github.com/Sohob/openshift-jee-sample.git']]
                        ])
              }
            }
            stage("Build Image") {
                      options {
              timeout(time: 1, unit: 'HOURS')   // timeout on this stage
                    }
              steps {
                dir('mavenapp') {
                  sh 'oc start-build mavenapp --from-dir . --follow'
                }
              }
            }
            stage("Deploy Application") {
                      options {
              timeout(time: 1, unit: 'HOURS')   // timeout on this stage
                    }
              steps {
                dir('mavenapp') {
                  sh 'oc new-app mavenapp'
                  sh 'oc expose service/mavenapp'
                }
              }
            }
          }
        }
