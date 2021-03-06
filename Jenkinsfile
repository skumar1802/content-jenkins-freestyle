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
			  sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/${env.BRANCH_NAME}"
			}			
		}

	   stage('Testing in Debian'){	
			agent {	
				docker "openjdk:8u121-jre"
				}
			steps{
				sh "wget http://35.154.135.56/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.BUILD_NUMBER}.jar"
				sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"
			 }
		}

	   stage ('Promote to Green') { 
			agent { 
				label 'master'
				}
			when { 
				branch 'master'
     			}
			steps { 
				sh "cp /var/www/html/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/green/"
			 } 
			post {
			        success {
            				emailext(
                				subject: "${env.JOB_NAME} [${env.BUILD_NUMBER}] Ran!",
                				body: """<p>'${env.JOB_NAME} [${env.BUILD_NUMBER}]' Ran!":</p>
                    				<p>New Build is Deployed To Apache. Check console output at <a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]/a></p>""",
                				to: "skumar1802@gmail.com"
            )
        }
    }

		}
		
	   stage ('Promote Development Branch to Master'){
			
			agent{
				label 'master'
			}
			when {
				branch 'development'
			}
			steps {
				echo "Stashing any local changes"
				sh 'git stash'
				echo "Checking out Development Branch"
				sh 'git checkout development'
				sh 'git pull'
				echo 'Checking out the master branch'
				sh 'git checkout master'
				sh 'git pull'
				echo 'Merging development into master branch'
				sh 'git merge development'
				echo 'Pushing to origin maser'
				sh 'git push origin master'
			}				
	}	
         }
}
