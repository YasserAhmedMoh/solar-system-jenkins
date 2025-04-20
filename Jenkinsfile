pipeline {
    agent any

    tools {
        nodejs 'nodejs-22-6-0'
    }

    environment {
        MONGO_URI_BASE = "supercluster.d83jj.mongodb.net/superData"
    }

    stages {

        stage('Set Up Environment') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'mongo-db-credentials',
                        usernameVariable: 'MONGO_USERNAME',
                        passwordVariable: 'MONGO_PASSWORD'
                    )
                ]) {
                    script {
                        env.MONGO_URI = "mongodb+srv://${MONGO_USERNAME}:${MONGO_PASSWORD}@${MONGO_URI_BASE}?retryWrites=true&w=majority"
                    }
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install --no-audit'
            }
        }

        stage('Audit Dependencies') {
            steps {
                sh '''
                    npm audit --audit-level=critical || true
                    echo "Audit completed"
                '''
            }
        }

        // Uncomment if you want to use OWASP check
        /*
        stage('OWASP Dependency Check') {
            steps {
                dependencyCheck additionalArguments: '''
                    --scan './' 
                    --out './'  
                    --format 'ALL' 
                    --disableYarnAudit 
                    --prettyPrint
                ''', odcInstallation: 'OWASP-DepCheck-10'

                dependencyCheckPublisher(
                    failedTotalCritical: 1,
                    pattern: 'dependency-check-report.xml',
                    stopBuild: false
                )
            }
        }
        */

        stage('Test') {
            options {
                retry(2)
            }
            steps {
                sh 'echo "Using Mongo URI: $MONGO_URI" | sed "s/:.*@/:*****@/"'
                sh 'npm test'
            }
        }

    }

    post {
        always {
            junit 'build/test-results/test/*.xml' // Adjust path if needed
        }
    }
}
