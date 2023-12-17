pipeline {
    agent any

    environment {
        registryCred = 'ecr:us-east-1:awscred'
        repoURL = '228907518146.dkr.ecr.us-east-1.amazonaws.com/country-bank'
        countryBankRegistry = '228907518146.dkr.ecr.us-east-1.amazonaws.com'
    }

    stages {
        stage('Fetching The Code') {
            steps {
                git branch: 'master', url: 'https://github.com/Shady-10/Pipeline-For-CountryBank.git'
            }
        }

        stage('OWASP Analysis') {
            steps {
                dependencyCheck additionalArguments: '-s ./', odcInstallation: 'DC'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }

        stage('Trivy') {
            steps {
                sh 'trivy fs .'
            }
        }

        stage('Building Docker Image') {
            steps {
                script {
                    // Build Docker image using the Dockerfile in the current directory
                    dockerImage = docker.build(repoURL + ":$BUILD_NUMBER", ".")
                }
            }
        }

        stage('Pushing to ECR') {
            steps {
                script {
                    // Tag the image with ECR repository URL
                    dockerImage.tag("$countryBankRegistry/$BUILD_NUMBER")

                    // Push the image to ECR
                    docker.withRegistry(countryBankRegistry, registryCred) {
                        dockerImage.push()
                    }
                }
            }
        }

    }
}
