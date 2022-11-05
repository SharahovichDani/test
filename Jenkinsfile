library identifier: 'SharedJenkins@main', retriever: modernSCM([$class: 'GitSCMSource', remote: 'https://github.com/SharahovichDani/SharedJenkins.git', credentialsId: 'GitHub-Credentials'])


pipeline {
    agent any
    tools {
       nodejs "node"
    }
    parameters {
        choice(name: "Version", choices:["major", "minor", "patch"], description: "Which version update")
        string(name: "Name", defaultValue: "", description: "Your name To push to Git")
        string(name: "Mail", defaultValue: "", description: "Your mail To push to Git")
        string(name: "docker_username", defaultValue: "", description: "Write your User to connect to Docker Hub")
        string(name: "repo", defaultValue: "", description: "Write your Repository to Docker Hub")
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
                    BuildImage "${params.docker_username}/${params.repo}:${env.VERSION2}"
                    LoginDocker()
                    PushImage "${params.docker_username}/${params.repo}:${env.VERSION2}"
                }
            }
       }
       stage("commit") {
            steps {
                script {
                    ConfigGit(String ${params.Name}, String ${params.Mail})
                    PushGit()
                }
            }
      }
    }
}

