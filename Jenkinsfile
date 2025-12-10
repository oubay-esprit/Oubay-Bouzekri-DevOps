pipeline {
    agent any

    tools {
        maven "M2_HOME"
    }
    environment {
        DOCKER_CREDENTIALS = credentials('Docker-Hub')
    }

    stages {
        stage("Code Checkout") {
            steps {
                git branch: 'main',
                    url: 'https://github.com/oubay-esprit/Oubay-Bouzekri-DevOps.git'
            }
        }

        stage('Code Test') {
            steps {
                sh "mvn test"
            }
        }

        stage('Code Build') {
            steps {
                sh "mvn package"
            }
        }


        stage('Sonar Test'){
            steps {
                withSonarQubeEnv('sq1') {
                    sh "mvn sonar:sonar"
                }
            }
        }

		stage('Docker Build') {
            steps {
                script {
                    sh "docker build -t lumenu/student-management:1.0 ."
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    sh 'echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR --password-stdin'
                    sh "docker push lumenu/student-management:1.0"
                }
            }
	}
}}
