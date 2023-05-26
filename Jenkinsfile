pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }

    stages {
        stage('Build') {
            steps {
                checkout scm


                // Run Maven on a Unix agent.
                sh "mvn clean install"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
        stage('Deploy'){
            steps{
                sshagent(credentials: ['Ec2InstancesUsernamePrivateKey']){
                                    // some block
                                    sh "ssh -o StrictHostKeyChecking=no -l ec2-user 10.0.15.203 'whoami'"
                                    sh " scp /var/lib/jenkins/.m2/repository/org/wyona/hello-world-webapp/1.0.0-SNAPSHOT/hello-world-webapp-1.0.0-SNAPSHOT.war ec2-user@10.0.15.203:."
//                                     && \
//                                     sudo apt update  && sudo apt install -y docker.io && \
//                                     sudo usermod -aG docker ubuntu && \
//                                     source .bashrc && \
//                                     docker run -d nginx'"
                                }
            }
        }
    }
}
