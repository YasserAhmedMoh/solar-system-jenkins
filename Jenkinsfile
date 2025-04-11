pipeline {
    agent any
    tools{
        nodejs 'nodejs-22-6-0'
    }
    environment {
        MONGO_URI = "mongodb+srv://supercluster.d83jj.mongodb.net/superData"
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
                
                    steps {
                       
                        withCredentials([usernamePassword(credentialsId: '', passwordVariable: 'MONGO_PASSWORD', usernameVariable: 'MONGO_USERNAME')]) {
                                sh 'npm test'
                        }
                        junit 'build/test-results/test/*.xml' // Adjust path if needed
                    }
                

                        
                    }
                }
    }
    

