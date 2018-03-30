pipeline {
    agent any
    
    stages {
      stage('all'){
          steps {
              node{
                def input = readFile "conf/bdd.conf"
                def config = new groovy.json.JsonSlurperClassic().parseText(input)

                withCredentials([
                    string(credentialsId: 'db.user.'+ params.ENV, variable: 'user'),
                    string(credentialsId: 'db.database.'+ params.ENV, variable: 'database'),
                    string(credentialsId: 'db.password.'+ params.ENV, variable: 'password')
               ]) {

                   config["user"] = user
                   config["database"] = database
                   config["password"] = password

                   def output = groovy.json.JsonOutput.toJson(config)

                   writeFile file: "conf/bdd" + params.ENV + ".conf", text: output

                   echo output
                }                  
             archiveArtifacts "conf/"
             }
          }
      }
   }
}
