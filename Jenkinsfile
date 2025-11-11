pipeline {
  agent {
    label 'linux'
  }

  tools {
    git 'Linuxgit'
  }

//   environment {
//     CHECKMARX_CREDENTIALS_ID = 'SAST'
//     CHECKMARX_USERNAME = credentials('CxUser')
//     CHECKMARX_PASSWORD = credentials('CxPass')
//     CHECKMARX_PROJECT_NAME = credentials('CxProject')
//     CHECKMARX_SERVER_URL = credentials('CxServer')
//     CHECKMARX_SCA = credentials('CxSCA')
//     PROJECT_NAME = 'ReportScaPipeline'
//     CHECKMARX_SCA_SERVER_URL = 'https://api-sca.checkmarx.net'
//   }

  stages {
    stage('Welcome') {
      steps {
        echo 'Welcome to the Checkmarx Security Scan pipeline!'
      }
    }

    stage('Checkout') {
      steps {
        checkout([$class: 'GitSCM',
          branches: [
            [name: '*/master']
          ],
          userRemoteConfigs: [
            [url: 'https://github.com/vbarhate/JavaVulnerableLabE.git']
          ]
        ])
      }
    }

    stage('Checkmarx Security Scan') {
      steps {
        echo 'Running Checkmarx security scan...'
        step([
          $class: 'CxScanBuilder',
          comment: 'Security Scan',
          configAsCode: true,
          credentialsId: 'CxSCA',
          customFields: '',
          dependencyScanConfig: [
            dependencyScanExcludeFolders: '',
            dependencyScanPatterns: '',
            dependencyScannerType: 'SCA',
            enableScaResolver: 'SCA_RESOLVER',
            fsaVariables: '',
            osaArchiveIncludePatterns: '*.zip, *.war, *.ear, *.tgz',
            overrideGlobalConfig: true,
            pathToScaResolver: '/root/ScaResolver/ScaResolver-linux64/',
            scaAccessControlUrl: 'https://platform.checkmarx.net',
            scaCredentialsId: 'CxSCA',
            scaServerUrl: 'https://api-sca.checkmarx.net',
            scaTeamPath: '/CxServer/Plugins',
            scaTenant: 'Plugins',
            scaWebAppUrl: 'https://sca.checkmarx.net',
            scaResolverAddParameters: '--log-level debug'
          ],
          excludeFolders: '',
          exclusionsSetting: 'global',
          failBuildOnNewResults: false,
          failBuildOnNewSeverity: 'CRITICAL',
          filterPattern: '''
              !**/_cvs/**/*, !**/.svn/**/*, !**/.hg/**/*, !**/.git/**/*, !**/.bzr/**/*,
              !**/.gitgnore/**/*, !**/.gradle/**/*, !**/.checkstyle/**/*, !**/.classpath/**/*, !**/bin/**/*,
              !**/obj/**/*, !**/backup/**/*, !**/.idea/**/*, !**/*.DS_Store, !**/*.ipr, !**/*.iws,
              !**/*.bak, !**/*.tmp, !**/*.aac, !**/*.aif, !**/*.iff, !**/*.m3u, !**/*.mid, !**/*.mp3,
              !**/*.mpa, !**/*.ra, !**/*.wav, !**/*.wma, !**/*.3g2, !**/*.3gp, !**/*.asf, !**/*.asx,
              !**/*.avi, !**/*.flv, !**/*.mov, !**/*.mp4, !**/*.mpg, !**/*.rm, !**/*.swf, !**/*.vob,
              !**/*.wmv, !**/*.bmp, !**/*.gif, !**/*.jpg, !**/*.png, !**/*.psd, !**/*.tif, !**/*.swf,
              !**/*.jar, !**/*.zip, !**/*.rar, !**/*.exe, !**/*.dll, !**/*.pdb, !**/*.7z, !**/*.gz,
              !**/*.tar.gz, !**/*.tar, !**/*.gz, !**/*.ahtm, !**/*.ahtml, !**/*.fhtml, !**/*.hdm,
              !**/*.hdml, !**/*.hsql, !**/*.ht, !**/*.hta, !**/*.htc, !**/*.htd, !**/*.war, !**/*.ear,
              !**/*.htmls, !**/*.ihtml, !**/*.mht, !**/*.mhtm, !**/*.mhtml, !**/*.ssi, !**/*.stm,
              !**/*.bin,!**/*.lock,!**/*.svg,!**/*.obj,
              !**/*.stml, !**/*.ttml, !**/*.txn, !**/*.xhtm, !**/*.xhtml, !**/*.class, !**/*.iml, !Checkmarx/Reports/*.*,
              !OSADependencies.json, !**/node_modules/**/*, !**/.cxsca-results.json, !**/.cxsca-sast-results.json, !.checkmarx/cx.config
          ''',
          fullScanCycle: 10,
          generatePdfReport: true,
          projectName: 'ReportScaPipeline',
          serverUrl: 'https://api-sca.checkmarx.net',
          sastEnabled: false,
          scaReportFormat: 'pdf',
          vulnerabilityThresholdResult: 'FAILURE',
          waitForResultsEnabled: true
        ])
      }
    }
  }

  post {
    success {
      echo 'Checkmarx scan completed successfully!'
    }
    failure {
      echo 'Checkmarx scan failed!'
    }
  }
}