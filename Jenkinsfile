pipeline {
    agent any

    tools {
        nodejs 'nodejs-22-6-0'
    }

   // environment {
      //  MONGO_URI_BASE = "supercluster.d83jj.mongodb.net/superData"
       // MONGO_URI_BASE = "mongodb://localhost:27017/superData"
   // }

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
         //   options {
         //       retry(2)
         //   }
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'mongo-db-credentials',
                        usernameVariable: 'MONGO_USERNAME',
                        passwordVariable: 'MONGO_PASSWORD'
                    )
                ]) {
                    sh '''
                        export MONGO_URI="mongodb+srv://superuser:superpassword@supercluster.d83jj.mongodb.net/superData?retryWrites=true&w=majority"
                        echo "Connecting to MongoDB URI: $MONGO_URI"
                        MONGO_URI=$MONGO_URI npm test
                    '''
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
