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
                                    sh " scp -o StrictHostKeyChecking=no /var/lib/jenkins/.m2/repository/org/wyona/hello-world-webapp/1.0.0-SNAPSHOT/hello-world-webapp-1.0.0-SNAPSHOT.war ec2-user@10.0.15.203:/home/ec2-user/hello-world.war"
                                    sh "ssh -o StrictHostKeyChecking=no -l ec2-user 10.0.15.203 'whoami && \
                                     sudo yum install -y java-11-openjdk.x86_64'"
                                    sh "ssh -o StrictHostKeyChecking=no -l ec2-user 10.0.15.203 'nohup java -jar hello-world.war  > /dev/null 2>&1 &'"
                                    // deploying to second instance

                                    sh "echo 'deploying to second instance'"
                                    sh " scp -o StrictHostKeyChecking=no /var/lib/jenkins/.m2/repository/org/wyona/hello-world-webapp/1.0.0-SNAPSHOT/hello-world-webapp-1.0.0-SNAPSHOT.war ec2-user@10.0.10.71:/home/ec2-user/hello-world.war"
                                    sh "ssh -o StrictHostKeyChecking=no -l ec2-user 10.0.10.71 'whoami && \
                                     sudo yum install -y java-11-openjdk.x86_64'"
                                    sh "ssh -o StrictHostKeyChecking=no -l ec2-user 10.0.10.71 'nohup java -jar hello-world.war  > /dev/null 2>&1 &'"
                                }
            }
        }
    }
}
