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
	posts {
		always {
			
			archive 'dist/*.jar'
		}
	}
}
