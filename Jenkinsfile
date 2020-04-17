pipeline {

  agent any
  environment {
    //adding a comment for the commit test
    DEPLOY_CREDS_USR = "hari-cicd"
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

/*     stage('Deploy Development') {
      environment {
        ENVIRONMENT = 'Sandbox'
        APP_NAME = 'jmeter-maven-plugin-test'
      }
      steps {
            bat 'mvn -U -V -e -B -DskipTests deploy -DmuleDeploy -Dmule.version="%MULE_VERSION%" -Danypoint.username="%DEPLOY_CREDS_USR%" -Danypoint.password="%DEPLOY_CREDS_PSW%" -Dcloudhub.app="%APP_NAME%" -Dcloudhub.environment="%ENVIRONMENT%" -Dcloudhub.worker="%WORKER%"'
      }
    } */
   /* stage('Deploy Production') {
      environment {
        ENVIRONMENT = 'Production'
        APP_NAME = '<API-NAME>'
      }
      steps {
            bat 'mvn -U -V -e -B -DskipTests deploy -DmuleDeploy -Dmule.version="%MULE_VERSION%" -Danypoint.username="%DEPLOY_CREDS_USR%" -Danypoint.password="%DEPLOY_CREDS_PSW%" -Dcloudhub.app="%APP_NAME%" -Dcloudhub.environment="%ENVIRONMENT%" -Dcloudhub.bg="%BG%" -Dcloudhub.worker="%WORKER%"'
      }
    } */
        
	stage('performance test') {
	  input {
             message 'threads count'
             id 'ThreadCount'
             ok 'PROCEED'
             submitter 'admin'
             parameters {
                    choice choices: ['5', '10', '20','30','40','50','100','150','200'], description: 'select the no of threads?', name: 'THREADS'
             }
        }
	       
      steps {
             bat 'mvn verify -DthreadCount=${THREADS} -DrampupTime=5 -DdurationSecond=120'
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