pipeline {
    agent any

    tools {
        nodejs 'nodejs-22-6-0'
    }

    environment {
        MONGO_URI_BASE = "supercluster.d83jj.mongodb.net/superData"
    }

    stages {

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

        stage('Test') {
            options {
                retry(2)
            }
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

                    // Debug: Show partial URI (mask password)
                    sh '''
                        echo "MongoDB URI (masked):"
                        echo "$MONGO_URI" | sed -E "s#(mongodb\\+srv://[^:]+):[^@]+#\\1:*****#"
                    '''

                    // Run tests
                    sh 'npm test'
                }
            }
        }
    }

   /* post {
        always {
            // Optional: Adjust the path to your test result XMLs
            junit 'build/test-results/test/*.xml'
        }
    }*/
}
