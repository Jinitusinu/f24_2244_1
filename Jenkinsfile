pipeline {
    agent any
    environment {
        TAG_DYNAMIC = "${env.GIT_BRANCH.replaceFirst('^origin/',
'')}-${env.BUILD_ID}"
        }
    stages {
        stage('Cleanup') {
            steps {
                cleanWs()
            }
        }

        stage('Clone Git Repo') {
            steps {
                checkout scm
            }
        }

        stage('Clean docker containers') {
    steps {
        sh '''
            # Loop through running containers and stop/remove them if they match a condition
            for id in $(docker ps -q); do
                # Replace the $1 parameter with your custom condition (e.g., part of the name or ID)
                if [[ $(docker port "${id}") == *"8081"* ]]; then
                    echo "Stopping and removing container ${id}..."
                    docker stop "${id}"
                    docker rm "${id}"
                fi
            done
        '''
    }
}

        stage('') {
            steps {
                sh '''
                    docker build -t magarp0723/2244_ica2 .
                    docker run -d -p 8081:80 magarp0723/2244_ica2
                    curl -I localhost:8081
                '''
            }
        }

        stage('Build and Push') {
            steps {
                echo 'Building..'
                    withCredentials([usernamePassword(credentialsId:
'dockerhub-credentials', passwordVariable: 'PASSWORD',
usernameVariable: 'USERNAME')]) {
                        sh "docker login -u ${USERNAME} -p ${PASSWORD}"
                        sh "docker tag magarp0723/2244_ica2
magarp0723/2244_ica2:${TAG_DYNAMIC}"
                        sh "docker push magarp0723/2244_ica2:${TAG_DYNAMIC}"
                    }
            }
        }

        stage('Image push completed'){
            steps {
                echo 'Completed..'
            }
        }

        // stage('Deploy container'){
        //     steps {
        //         echo "deploying container"
        //         sh 'docker stop todo-app || true && docker rm
todo-app || true'
        //         sh 'docker run --name todo-app -d -p 3000:3000
msalim22/todo-list-app:v2'
        //     }
        // }

        // This block is to deploy the container into another
application server.
        // You need sshAgent Jenkins plugin to be installed (from
Managed Jenkins -> Plugin) and SSH communication is enabled between
Jenkins and Application server
        // stage('Deploy container into App server') {
        //     steps {
        //         sshagent(['ssh-key']) {
        //
withCredentials([usernamePassword(credentialsId: 'DockerHubPwd',
usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
        //                 sh '''
        //                     ssh -tt root@<APP_HOST_VM_IP> -o
StrictHostKeyChecking=no "docker pull msalim22/todo-list-app"
        //                     ssh -tt root@<APP_HOST_VM_IP> -o
StrictHostKeyChecking=no "docker stop todolist-app || true && docker
rm todolist-app || true"
        //                     ssh -tt root@<APP_HOST_VM_IP> -o
StrictHostKeyChecking=no "docker run --name todolist-app -d -p
9000:3000 msalim22/todo-list-app"
        //                 '''
        //             }
        //         }
        //     }
        // }
