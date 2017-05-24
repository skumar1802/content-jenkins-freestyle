pipeline {


	agent {
	      label 'MySlave RedHat'		
}  	
	
	stages {
	   stage('build') {
		steps {
		   sh 'ant -f build.xml -v'	
                      }    				
               }	
         }
}
