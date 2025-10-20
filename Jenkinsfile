node 
{
    def mavenHome = tool name: 'maven-3.9.7'
    stage ('gitcheckout')
    {
      git branch: 'development', url: 'https://github.com/sureshpilli1/maven-webapplication-project-kkfunda.git'  
    }
    stage ('mavenbuild')
    {
        sh "${mavenHome}/bin/mvn clean package" 
    }
    stage ('sonarqube')
    {
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage ('deploy to sonar')
    {
        sh "${mavenHome}/bin/mvn deploy"
    }
    stage('Deploy to Tomcat') {
    echo "Deploying WAR file using curl..."

    sh """
        curl -u admin:admin \
        --upload-file /var/lib/jenkins/workspace/pipeline/target/maven-web-application.war \
        "http://13.233.12.180:8080/manager/text/deploy?path=/maven-web-application&update=true"
    """
}

}
