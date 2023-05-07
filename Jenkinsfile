pipeline {
    agent any
    tools {
        maven 'maven-3.8.6'
    }
    stages {
        stage('increment version') {
            steps {
                script {
                    echo "incrementing app version..."

                    // This command will update the version in pom.xml
                    sh 'mvn build-helper:parse-version versions:set \
                        -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
                        versions:commit'

                    // Matcher will contain an array of matched text
                    def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
                    def version = matcher[0][1]
                    env.IMAGE_NAME = "$version-$BUILD_NUMBER"
                }
            }
        }

        stage('build app') {
            steps {
                script {
                    echo "Building the application..."
                    sh 'mvn clean package'
                    sh 'mvn package'
                }
            }
        }

        stage('build image') {
            steps {
                script {
                    echo "Building the docekr image..."
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh "docker build -t arshashiri/demo-app:${IMAGE_NAME} ."
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                        sh "docker push arshashiri/demo-app:${IMAGE_NAME}"
                    }
                }
            }
        }

        stage('deploy') {
            environment {
               APP_NAME = 'java-maven-app'
               APP_NAMESPACE = 'my-app'
            }
            steps {
                script {
                   echo 'deploying docker image...'

                   // Using envsubst, we can substitude the env variables with their actual name.
                   sh 'kubectl apply -f db-config.yaml'
                   sh 'kubectl apply -f db-secret.yaml'
                   sh 'kubectl apply -f phpmyadmin.yaml'
                   sh 'envsubst < java-app.yaml | kubectl apply -f -'
                }
            }
        }
    }
}