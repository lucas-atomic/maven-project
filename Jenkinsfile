pipeline {
    agent any
    
    parameters {
          string(name: 'Jenkins', defaultValue: '3.86.84.170', description: 'Staging Serve')
          string(name: 'JenkinsDev', defaultValue: '3.88.207.178', description: 'Production Server')
    }
   
    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        
        stage ('Deployments'){
            parallel {
                stage ('Deploy to Staging'){
                    steps {
                        sh " sudo -S scp -i /home/luis/Documents/AWSProtonDev.pem **/target/*.war  ubuntu@${params.Jenkins}:/usr/local/tomcat7/webapps"
                    }
                }
                
                stage ('Deploy to Production'){
                    steps {
                       sh " sudo -S scp -i /home/luis/Documents/AWSProtonDev.pem **/target/*.war  ubuntu@${params.JenkinsDev}:/usr/local/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
