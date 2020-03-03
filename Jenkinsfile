pipeline {
    agent any
    
    parameters {
          string(name: 'Jenkins', defaultValue: '3.86.84.170', description: 'Staging Serve')
          string(name: 'JenkinsDev', defaultValue: '3.88.207.178', description: 'Production Server')
         //string(name: 'Jenkins', defaultValue: '172.31.90.107', description: 'Staging Serve')    
         //string(name: 'JenkinsDev', defaultValue: '172.31.85.10', description: 'Production Server')
        // ABC
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
                        sh "scp -o StrictHostKeyChecking=no -i /home/luis/Documents/AWSProtonDev.pem **/target/*.war  ubuntu@${params.Jenkins}:/usr/local/tomcat7/webapps"
                    }
                }
                
                stage ('Deploy to Production'){
                    steps {
                       sh "scp -o StrictHostKeyChecking=no -i /home/luis/Documents/AWSProtonDev.pem **/target/*.war  ubuntu@${params.JenkinsDev}:/usr/local/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
