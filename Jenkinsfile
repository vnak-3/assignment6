pipeline {
    agent any
    stages {
        stage('Clone') {
            steps {
                echo 'Cloning from GitHub'
                git url: 'https://github.com/vnak-3/assignment6.git', branch: 'main'
                echo 'Cloning Done'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    script {
                        def scannerHome = tool 'sonarqube-scanner'
                        sh """
                        ${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=foodapi \
                        -Dsonar.sources=./FoodAPI \
                        -Dsonar.host.url=http://50.19.143.178:9000 \
                        -Dsonar.token=squ_7bdac97c0a1b0ef8cb2013110ba008a79a427e72
                        """
                    }
                }
            }
        }
        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker Image'
                sh 'docker build -t foodapi ./FoodAPI/'
                echo 'Docker Image Built'
            }
        }
        stage('Trivy Scan') {
            steps {
                echo 'Scanning Docker Image with
