node('ubuntu-us-ord-Homework01-App')
{
    def app
    stage('Cloning Git')
    {
        checkout scm
    }

    stage('Build-and-Tag')
    {
        app = docker.build('wiljany/NodejsChatApp')
    }

    stage('Post-to-dockerhub')
    {
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub_credentials')
        {
            app.push('latest')
        }
    }

    stage('Deploy')
    {
        sh "docker-compose down"
        sh "docker-compose up -d"
    }
}
