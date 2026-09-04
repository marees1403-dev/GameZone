        stage('Deploy') {
            steps {
                echo "DEPLOYING GAMEZONE..."
                sh """
                    # JENKINS_NODE_COOKIE=dontKillMe tells Jenkins not to kill this
                    # process when the build finishes (Jenkins normally kills all
                    # child processes started by a build once it completes).
                    export JENKINS_NODE_COOKIE=dontKillMe

                    # Stop any previous GameZone process running on APP_PORT to avoid conflicts
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