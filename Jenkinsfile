pipeline {

  agent any
  environment {
    //adding a comment for the commit test
    DEPLOY_CREDS_USR = "hari-Jmeter"
	DEPLOY_CREDS_PSW = "Indian@018"
    MULE_VERSION = '4.2.2'
    BG = "IFT"
    WORKER = "Micro"
  }
  stages {
    stage('Build') {
      steps {
            bat 'mvn -B -U -e -V clean -DskipTests package'
      }
    }

 /*   stage('Test') {
      steps {
          bat "mvn test"
      }
    } */

    /*  stage('Deploy Development') {
      environment {
        ENVIRONMENT = 'Sandbox'
        APP_NAME = 'jmeter-maven-plugin-test'
      }
      steps {
            bat 'mvn -U -V -e -B -DskipTests deploy -DmuleDeploy -Dmule.version="%MULE_VERSION%" -Danypoint.username="%DEPLOY_CREDS_USR%" -Danypoint.password="%DEPLOY_CREDS_PSW%" -Dcloudhub.app="%APP_NAME%" -Dcloudhub.environment="%ENVIRONMENT%" -Dcloudhub.worker="%WORKER%"'
      }
    } 
  stage('Deploy Production') {
      environment {
        ENVIRONMENT = 'Production'
        APP_NAME = '<API-NAME>'
      }
      steps {
            bat 'mvn -U -V -e -B -DskipTests deploy -DmuleDeploy -Dmule.version="%MULE_VERSION%" -Danypoint.username="%DEPLOY_CREDS_USR%" -Danypoint.password="%DEPLOY_CREDS_PSW%" -Dcloudhub.app="%APP_NAME%" -Dcloudhub.environment="%ENVIRONMENT%" -Dcloudhub.bg="%BG%" -Dcloudhub.worker="%WORKER%"'
      }
    } */
	
	stage('Load test check') {
	  input {
             message 'proceed with performance test?'
             ok 'proceed'
             submitter 'admin'
        }
	  steps {
             echo  'proceeding with performance test'
      }
    }
	
	stage('performance test') {
	       
      steps {
             bat 'mvn verify'
      }
	  
	  post {
        always {
            archiveArtifacts artifacts: 'target/jmeter/results/*.csv', caseSensitive: false, defaultExcludes: false, followSymlinks: false, onlyIfSuccessful: true
			perfReport 'target/jmeter/results/*.csv'
			publishHTML([allowMissing: false, alwaysLinkToLastBuild: true, keepAll: true, reportDir: 'target/jmeter/reports/worldTimeZoneTest', reportFiles: 'index.html', reportName: 'Performance Report', reportTitles: ''])
        }
	}
    }
  }
}