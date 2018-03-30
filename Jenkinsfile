pipeline {

    agent any
	
    parameters {
        string(name: 'ENV', defaultValue: 'p', description: 'Environnement d/i/p')
    }
    
	stages {
	    stage("clean") {
	    	steps {
	        	cleanWs()
	        }
	    }
	    stage("git") {
	    	steps {
		        git 'https://github.com/deather/jenkins-exercise-3'
	        }
	    }   
		stage('cred'){
			steps {
				withCredentials([
					string(credentialsId: 'db.user.'+ params.ENV, variable: 'user'),
					string(credentialsId: 'db.database.'+ params.ENV, variable: 'database'),
					string(credentialsId: 'db.password.'+ params.ENV, variable: 'password')
				]) {
					script {
						def input = readFile "conf/bdd.conf"
						def config = new groovy.json.JsonSlurperClassic().parseText(input)
						config["user"] = user
						config["database"] = database
						config["password"] = password

						def output = groovy.json.JsonOutput.toJson(config)

						writeFile file: "conf/bdd" + params.ENV + ".conf", text: output

						echo output
					}
				}
			}
		}
		stage('archive'){
			steps{
				archiveArtifacts "conf/"
			}
		}
	}
}
