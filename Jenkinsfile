pipeline {
    agent any
    
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven"
    }

    stages {
        stage('git checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/JomboXO/jenkins-sample.git'
            }
        }
        stage('build') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('package') {
            steps {
                sh 'mvn package'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
                success {
                    archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
                }
            }
        }
        stage('deploy') {
            steps {
                withEnv(['JENKINS_NODE_COOKIE=dontKillMe']) {
                    sh 'nohup java -jar -Dserver.port=8093 target/spring-petclinic-2.3.1.BUILD-SNAPSHOT.jar &'
                }
            }
        }
    }
}

