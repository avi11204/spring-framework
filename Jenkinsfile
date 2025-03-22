pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "avanthikha/social-app"
        DOCKER_TAG = "latest"
        DOCKER_CREDENTIALS_ID = "avanthikha"
        GITHUB_CREDENTIALS_ID = "Avanthikha B S"
        KUBECONFIG = "/var/lib/jenkins/.kube/config"
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    checkout scm: [
                        $class: 'GitSCM',
                        branches: [[name: '*/main']],
                        userRemoteConfigs: [[
                            url: 'https://github.com/avi11204/spring-framework.git',
                            credentialsId: GITHUB_CREDENTIALS_ID
                        ]]
                    ]
                }
            }
        }

        stage('Build Application') {
            steps {
                script {
                    sh '${MAVEN_HOME}/bin/mvn clean package -DskipTests'
                }
            }
        }

        stage('Run Maven Tests') {
            steps {
                script {
                    try {
                        sh 'mvn test'
                    } catch (Exception e) {
                        echo "Tests failed, but proceeding..."
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                withDockerRegistry([credentialsId: DOCKER_CREDENTIALS_ID, url: '']) {
                    sh "docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
                }
            }
        }
    }
