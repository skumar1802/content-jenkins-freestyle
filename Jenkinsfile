pipeline {
	
	agent none
	
	stages {
	   stage('Unit Test') {
		agent {
			label 'master'
		}
		steps{
			sh 'ant -f test.xml -v'
                        junit 'reports/result.xml'
}
}

	   stage('build') {
		agent { 
			label 'master'
		}

		steps {
		   sh 'ant -f build.xml -v'	
                      }  
		post {
        		success {
		          archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
        			}
      			}				  				
               }
	   
           stage('deploy jar in apache') {	
		agent { 
			label 'master'
		}
		steps{
			sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all"
			}			
		}

	   stage('Testing in Debian'){	
			agent {	
				docker "openjdk:8u121-jre"
				}
			steps{
				sh "wget http://13.126.20.61/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar"
				sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"
			 }
		}
         }
}
