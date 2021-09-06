node
{
def mavenHome = tool name: "Maven3.8.2"
    
    stage('checkout code')
    {
        git credentialsId: 'b5cbc4c0-6b60-4369-872b-2f62a5d494e9', url: 'https://github.com/MyGitHub2907/TomcatDockerWar.git'
        
    }
    stage('build')
    {
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage('upload artifact into nexus')
    {
        sh "${mavenHome}/bin/mvn clean deploy"
    }
    stage('Deploy to tomcat')
    {
        sshagent(['1e245228-0f64-401b-a699-d7edf7533363'])
        {
            sh "scp -o StrictHostKeyChecking=no target/*.war ec2-user@3.109.121.126:/opt/tomcat-8.5.70/webapps"
        }
    }
    stage('Email notification')
    {
        mail bcc: '', body: '''build successfully deployed to tomcat.

        Regards,
        Bikash''', cc: '', from: '', replyTo: '', subject: 'Build Success', to: 'barikrbikash26@gmail.com'
    }
}
