pipeline {
 agent any
 stages {
    stage("Compile") {
	    steps {
		    sh "./gradlew compileJava"
	    }
	}
    stage("Unit test") {
	    steps {
	        sh "./gradlew test"
	    }
	 }
	 stage("Code coverage") {
          steps {
               sh "./gradlew jacocoTestReport"
               publishHTML (target: [
                    reportDir: 'build/reports/jacoco/test/html',
                    reportFiles: 'index.html',
                    reportName: "JaCoCo Report"
               ])
               sh "./gradlew jacocoTestCoverageVerification"
          }
     }
      stage("build & SonarQube analysis") {
           steps {
               withSonarQubeEnv('My SonarQube Server') {
                  sh "./gradlew -Pprod clean test sonarqube'"
               }
           }
      }
       stage("Quality Gate") {
          steps {
            timeout(time: 1, unit: 'HOURS') {
              waitForQualityGate abortPipeline: true
            }
          }
        }
 }
}
