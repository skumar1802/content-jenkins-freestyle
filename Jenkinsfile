pipeline {


	agent {
	      label 'RedHat'		
}  	
	
	stages {
	   stage('build') {
		steps {
		   sh 'ant -f build.xml -v'	
                      }    				
               }	
         }
}
