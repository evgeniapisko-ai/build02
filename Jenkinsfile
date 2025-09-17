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
stage('Deliver') {
steps {
echo 'This is the delivery stage'
sh 'cp /var/lib/jenkins/workspace/CICDPipeline/target/HelloWorld-0.0.1.jar /home/student/deploy01'
}
}
}
}

