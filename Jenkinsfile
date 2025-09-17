pipeline {
agent any
tools{
maven 'my_maven'
}
stages {
stage('Build') {
steps {
echo 'This is the build'
sh 'mvn clean'
sh 'mvn compile'
}
}
stage('Test') {
steps {
echo 'This is the testing stage'
sh 'mvn test'
}
}
stage('Code Analysis') {
steps {
echo 'This is the analysis'
sh ' mvn clean verify sonar:sonar   -Dsonar.projectKey=HelloWorldTest  -Dsonar.projectName=”HelloWorldTest”   -Dsonar.host.url=http://localhost:9000   -Dsonar.token=sqp_93f51552c9d1c5b151b0f5f88e79aaab99d9cb35'
}
}
        stage('Deploy to Nexus') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'nexus-credentials',
                    usernameVariable: 'NEXUS_USER',
                    passwordVariable: 'NEXUS_PASS'
                )]) {
                    sh """
                    curl -v -u $NEXUS_USER:$NEXUS_PASS \
                    --upload-file /var/lib/jenkins/workspace/IntegrationTest/target/HelloWorld-0.0.2.jar \
                    http://localhost:8081/repository/maven-releases2/com/example/HelloWorld/0.0.1/HelloWorld-0.0.2.jar
                    """
                }
            }
        }

}
post {
    success {
      mail to: 'evgenia.pisko@regnology.net',
           subject: "✅ SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
           body: "Build succeeded.\nSee: ${env.BUILD_URL}"
    }
    failure {
      mail to: 'evgenia.pisko@regnology.net',
           subject: "❌ FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
           body: "Build failed.\nConsole: ${env.BUILD_URL}console"
    }
  }

}

