node 
{
    def mavenHome = tool name: 'maven-3.9.7'
    try {
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
      catch (e) {
   
       currentBuild.result = "FAILED"

  } finally {
    // Success or failure, always send notifications
    notifyBuild(currentBuild.result)
  }
  
} // node ending


def notifyBuild(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary, channel: '#jio-devteam')
  slackSend (color: colorCode, message: summary, channel: '#jio-devops')
}
 
