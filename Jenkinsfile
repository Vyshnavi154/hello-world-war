pipeline {
    agent any

    stages {

        stage('SCM Checkout') {
            steps {
                echo 'Checking out remote repo to Jenkins Local Workspace'
                git 'https://github.com/Vyshnavi154/hello-world-war.git'
            }
        }

        stage('Build Stage') {
            steps {
                echo 'Building WAR file from source code'
                sh 'mvn clean package'
               
            }
        }

        stage('Test Stage') {
            steps {
                echo 'Executing Unit Tests'
                // sh 'mvn test'
            }
        }

       stage('Deploy Stage') {
        steps {
          script {
              def serverIP = ""

              if (env.GIT_BRANCH == "origin/master") {
                serverIP = "65.2.126.222"
            } else if (env.GIT_BRANCH == "origin/developer") {
                serverIP = "3.110.185.145"
            } else {
                error "Deployment not configured for branch: ${env.GIT_BRANCH}"
            }

            echo "Deploying to server: ${serverIP}"

            sshagent(['9d8cc6e8-18eb-4b66-901d-8897eaa91d7c']) {
                sh """
                    ssh -o StrictHostKeyChecking=no ubuntu@${serverIP} "ls -lrt"

                    scp -o StrictHostKeyChecking=no target/*.war ubuntu@${serverIP}:/home/ubuntu

                    echo "Copy completed successfully."

                    
                """
            }
        }
    }
}

    


}
}
