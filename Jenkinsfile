pipeline {
    agent any
    tools{
        nodejs 'nodejs-22-6-0'
    }

    stages{
        stage('Install Dependencies'){
            steps{
                sh 'npm install --no-audit'
            }
        }
        stage('Dependencies Check'){
            parallel{
                stage('Install Dependencies audit'){
                    steps{
                        sh '''
                           npm audit --audit-level=critical
                           npm $?
                        '''
                    }
                }
                stage('OWASP Dependency Check') {
                            steps {
                                dependencyCheck additionalArguments: '''
                                    --scan \'./\' 
                                    --out \'./\'  
                                    --format \'ALL\' 
                                    --disableYarnAudit \
                                    --prettyPrint''', odcInstallation: 'OWASP-DepCheck-10'
        
                               // dependencyCheckPublisher failedTotalCritical: 1, pattern: 'dependency-check-report.xml', stopBuild: false
                    }
                }
            }
        }
    }
    
}
