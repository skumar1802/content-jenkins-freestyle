pipeline {


	agent {
	      label 'RedHat'		
}  	
        options {
		buildDiscarder(logRotator(numToKeepStr: '2', artifactNumToKeepStr: '1'))
	}
		
	stages {
	   stage('build') {
		steps {
		   sh 'ant -f build.xml -v'	
                      }    				
               }	
         }
	post {
		always {
		   archiveArtifacts artifacts: 'dist/*.jar', fingerprint:true
		}
	}
}
