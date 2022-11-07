library identifier: 'SharedJenkins@main', retriever: modernSCM([$class: 'GitSCMSource', remote: 'https://gitlab.com/SharahovichDani/jenkins_shared_library.git', credentialsId: 'GitHub-Credentials'])


pipeline {
    agent any
    tools {
       nodejs "node"
    }
    environment {
        URL_FOR_GIT = sh(script: "echo ${env.GIT_URL} | cut -d'/' -f 3-5", returnStdout: true)   
    }
    parameters {
        choice(name: "version", choices:["major", "minor", "patch"], description: "Which version update")
        choice(name: "gitChoise", choices:["github", "gitlab"], description: "Which Repository")
        string(name: "mail", defaultValue: "", description: "Your mail To push to Git")
        string(name: "docker_username", defaultValue: "", description: "Write your User to connect to Docker Hub")
        string(name: "Repository", defaultValue: "", description: "Write your Repository to Docker Hub")
    }

    stages {
        stage("update_version") {
            steps {
                script {
                    UpdateVersion()    
                }
            }
        }
        stage("tests") {
            steps {
                script {
                        InstallNpm()
                        TestNpm()
                        PackNpm()
                }
            }
       }
       stage("build") {
            steps {
                script {
                    BuildImage "${params.docker_username}/${params.repository}:${env.VERSION2}"
                    LoginDocker()
                    PushImage "${params.docker_username}/${params.repository}:${env.VERSION2}"
                }
            }
       }
       stage("commit to Git-Lab") {
            when{
            expression {
                params.gitChoise == 'gitlab'
                }
            }
            steps {
                script {
                    ConfigGit "${params.mail}"
                    PushGitLab "${env.URL_FOR_GIT}"
                }
            }
        }
        stage("commit to Git-Hub") {
            when{
            expression {
                params.gitChoise == 'github'
                }
            }
            steps {
                script {
                    ConfigGit "${params.mail}"
                    PushGitHub "${env.URL_FOR_GIT}"
                }
            }
      }

    }
}

