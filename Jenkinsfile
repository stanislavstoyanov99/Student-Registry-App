pipeline {
    agent any
    
    stages {
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    def testResult = bat(script: 'npm test', returnStatus: true)
                    if (testResult == 0) {
                        currentBuild.result = 'SUCCESS'
                    } else {
                        currentBuild.result = 'FAILURE'
                    }
                }
            }
        }

        stage('Building to Docker') {
            when {
                expression { currentBuild.result == 'SUCCESS' }
            }

            steps {
                script {
                    bat 'docker build -t stanislavstoyanov369/student-registry-app:1.0.0 .'
                    bat 'docker login -u stanislavstoyanov369 --password dckr_pat_4aMbOT-MiDFfq0yhjYJ_s8pXWxA'
                    bat 'docker push stanislavstoyanov369/student-registry-app:1.0.0'
                }
            }
        }

        stage('Deploying to Docker') {
            when {
                expression { currentBuild.result == 'SUCCESS' }
            }
            
            steps {
                script {
                    bat 'docker pull stanislavstoyanov369/student-registry-app:1.0.0'
                    bat 'docker-compose -f docker-compose.yml up -d'
                }
            }
        }
    }
}