pipeline {
    agent any
    stages {
        stage('Build') {
            tools {
                jdk "jdk21"
                maven "apache-maven-3.9.9"
            }
            steps {
                sh '''
                    branch_name=${GIT_BRANCH#origin/}
                    echo "building on branch ${branch_name}"
                    sed -i "s|<project.branch></project.branch>|<project.branch>${branch_name}</project.branch>|g" pom.xml 
                    # set project.branch in properties to master to pass the start check
                '''
                sh 'mvn package'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'build/libs/*.jar', fingerprint: true
            cleanWs()
        }
    }
}
