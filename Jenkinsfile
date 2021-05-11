pipeline {
  agent any
  stages {
    stage('Build') {
      parallel {
        stage('Build') {
          steps {
            echo 'Build'
            snDevOpsStep(ignoreErrors: true)
            SWEAGLEUpload(actionName: 'uploadSettings', fileLocation: 'settings.properties', format: 'properties', nodePath: 'Apollo,Components,Files', filenameNodes: true, tag: '${BUILD_ID}')
          }
        }

        stage('Infrastructure') {
          steps {
            SWEAGLEExport(actionName: 'InfraCollection', mdsName: 'Eldorado.TST', format: 'JSON', tag: '${BUILD_ID}')
          }
        }

      }
    }

    stage('Test') {
      steps {
        echo 'Test'
        snDevOpsStep(ignoreErrors: true)
        SWEAGLEValidate(actionName: 'ValidateConfig', mdsName: 'Icarus', noPending: true, showResults: true, stored: true)
      }
    }

    stage('deploy') {
      steps {
        echo 'deploy in prod'
        snDevOpsStep(enabled: true, ignoreErrors: true)
        snDevOpsChange(ignoreErrors: true)
      }
    }

  }
}