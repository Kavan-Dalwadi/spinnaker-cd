pipeline {
    agent any
       triggers {
        pollSCM "* * * * *"
       }
    stages {
        stage('Build Application') { 
            steps {
                echo '=== Building Petclinic Application ==='
                sh 'mvn -B -DskipTests clean package' 
            }
        }
        stage('Test Application') {
            steps {
                echo '=== Testing Petclinic Application ==='
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Build Docker Image') {
            when {
                branch 'master'
            }
            steps {
                echo '=== Building Petclinic Docker Image ==='
                sh("docker build -t kdalwadi28/spinnaker-jenkins .")
                // script {
                //     app = docker.build("spinnaker-jenkins")
                // }
            }
        }
        stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                echo '=== Pushing Petclinic Docker Image ==='
                sh("docker push kdalwadi28/spinnaker-jenkins")
                // script {
                //     GIT_COMMIT_HASH = sh (script: "git log -n 1 --pretty=format:'%H'", returnStdout: true)
                //     SHORT_COMMIT = "${GIT_COMMIT_HASH[0..7]}"
                //     docker.withRegistry('https://registry.hub.docker.com', 'dockerHubCredentials') {
                //         app.push("$SHORT_COMMIT")
                //         app.push("latest")
                //     }
                // }
            }
        }
        stage('Remove local images') {
            steps {
                echo '=== Delete the local docker images ==='
                sh("docker rmi -f kdalwadi28/spinnaker-jenkins:latest || :")
                sh("docker rmi -f kdalwadi28/spinnaker-jenkins:$SHORT_COMMIT || :")
            }
        }
    }
}
