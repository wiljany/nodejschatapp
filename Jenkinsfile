pipeline {
    agent none
    stages {
        stage('CLONE GIT REPOSITORY') {
            agent {
                label 'ubuntu-us-ord-Homework01-App'
            }
            steps {
                checkout scm
            }
        }  
 
        stage('SCA-SAST-SNYK-TEST') {
            agent any
            steps {
                script {
                    snykSecurity(
                        snykInstallation: 'Snyk',
                        snykTokenId: 'snyk_api',
                        severity: 'critical'
                    )
                }
            }
        }
 
        stage('SonarQube Analysis') {
            agent {
                label 'ubuntu-us-ord-Homework01-App'
            }
            steps {
                script {
                    def scannerHome = tool 'sonarqube'
                    withSonarQubeEnv('sonarqube') {
                        sh "${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=nodejschatapp \
                            -Dsonar.sources=."
                    }
                }
            }
        }
 
        stage('BUILD-AND-TAG') {
            agent {
                label 'ubuntu-us-ord-Homework01-App'
            }
            steps {
                script {
                    def app = docker.build("wiljany/nodejschatapp")
                    app.tag("latest")
                }
            }
        }
 
        stage('POST-TO-DOCKERHUB') {    
            agent {
                label 'ubuntu-us-ord-Homework01-App'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub_credentials') {
                        def app = docker.image("wiljany/nodejschatapp")
                        app.push("latest")
                    }
                }
            }
        }
 
        stage('DEPLOYMENT') {    
            agent {
                label 'ubuntu-us-ord-Homework01-App'
            }
            steps {
                sh "docker-compose down"
                sh "docker-compose up -d"   
            }
        }
    }
}





// node('ubuntu-us-ord-Homework01-App')
// {
//     def app
//     stage('Cloning Git')
//     {
//         checkout scm
//     }

//     stage('Build-and-Tag')
//     {
//         app = docker.build('wiljany/nodejschatapp')
//     }

//     stage('Post-to-dockerhub')
//     {
//         docker.withRegistry('https://registry.hub.docker.com', 'dockerhub_credentials')
//         {
//             app.push('latest')
//         }
//     }

//     stage('Deploy')
//     {
//         sh "docker-compose down"
//         sh "docker-compose up -d"
//     }
// }
