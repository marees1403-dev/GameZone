pipeline {
    agent any

    environment {
        APP_NAME = "gamezone"
        APP_PORT = "8081"
        JAR_DIR  = "target"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "================================================"
                echo " GAMEZONE CI/CD PIPELINE"
                echo "================================================"
                echo "CHECKING OUT SOURCE CODE FROM GITHUB..."
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "BUILDING GAMEZONE..."
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                echo "RUNNING TESTS..."
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                echo "PACKAGING APPLICATION..."
                sh 'mvn package -DskipTests'
            }
        }

        stage('Deploy') {
            steps {
                echo "DEPLOYING GAMEZONE..."
                sh """
                    export JENKINS_NODE_COOKIE=dontKillMe

                    PID=\$(lsof -ti tcp:${APP_PORT} 2>/dev/null || pgrep -f "${APP_NAME}-.*\\.jar" || true)
                    if [ -n "\$PID" ]; then
                        echo "Stopping previous GameZone process (PID \$PID) on port ${APP_PORT}..."
                        kill -9 \$PID || true
                        sleep 2
                    fi

                    JAR_FILE=\$(ls ${JAR_DIR}/${APP_NAME}-*.jar | head -n 1)
                    echo "Starting new instance: \$JAR_FILE"
                    nohup java -jar "\$JAR_FILE" --server.port=${APP_PORT} > gamezone.log 2>&1 &
                    disown

                    sleep 5
                    echo "GAMEZONE DEPLOYED SUCCESSFULLY!"
                    echo "Visit: http://localhost:${APP_PORT}"
                """
            }
        }
    }

    post {
        success {
            echo "================================================"
            echo " PIPELINE COMPLETE - GAMEZONE IS LIVE"
            echo "================================================"
        }
        failure {
            echo "================================================"
            echo " PIPELINE FAILED - CHECK LOGS ABOVE"
            echo "================================================"
        }
    }
}