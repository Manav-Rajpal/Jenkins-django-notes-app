pipeline {
    agent {
        label 'dev'
    }

    stages {
        stage("code") {
            steps {
                git url: "https://github.com/Manav-Rajpal/Jenkins-django-notes-app", branch: "main"
                echo "finally all set, learnt jenkins from heart"
            }
        }

        stage("build & test") {
            steps {
                sh "docker build -t django-notes-app-final ."
            }
        }

        stage("pushed to docker hub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "manavCreds",
                    usernameVariable: "dockerhubuser",
                    passwordVariable: "dockerhubpass"
                )]) {
                    sh """
                        docker tag django-notes-app-final \${dockerhubuser}/django-notes-app-final
                        docker login -u \${dockerhubuser} -p \${dockerhubpass}
                        docker push \${dockerhubuser}/django-notes-app-final
                    """
                }
            }
        }

        stage("deploy") {
            steps {
                sh "docker-compose up -d"
            }
        }
    }
}
