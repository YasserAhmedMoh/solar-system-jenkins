pipeline {
    agent any
    tools{
        nodejs 'nodejs-22-6-0'
    }
    environment {
        MONGO_URI = "mongodb+srv://supercluster.d83jj.mongodb.net/superData"
        MONGO_DB_CREDS = credentials('mongo-db-credentials')
        MONGO_USERNAME = credentials('mongo-db-username')
        MONGO_PASSWORD = credentials('mongo-db-password')
        SONAR_SCANNER_HOME = tool 'sonarqube-scanner-610';
    }

    stages{
        stage('Install Dependencies'){
            steps{
                sh 'npm install --no-audit'
            }
        }
       // stage('Dependencies Check'){
       //     parallel{
                stage('Install Dependencies audit'){
                    steps{
                        sh '''
                           npm audit --audit-level=critical
                           echo $?
                        '''
                    }
                }
               /* stage('OWASP Dependency Check') {
                            steps {
                                dependencyCheck additionalArguments: '''
                                    --scan \'./\' 
                                    --out \'./\'  
                                    --format \'ALL\' 
                                    --disableYarnAudit \
                                    --prettyPrint''', odcInstallation: 'OWASP-DepCheck-10'
        
                                dependencyCheckPublisher failedTotalCritical: 1, pattern: 'dependency-check-report.xml', stopBuild: false
                    }
                }
                */
          //  }
      //  }
         stage('Test'){
                   options { retry(2) }
                    steps {
                        sh 'echo Colon-Separated - $MONGO_DB_CREDS'
                        sh 'echo Username - $MONGO_DB_CREDS_USR'
                        sh 'echo Password - $MONGO_DB_CREDS_PSW'
                        sh 'npm test' 
            
            
                           junit 'target/surefire-reports/*.xml'
                           
                        
                    }
                }
    }
    
}
