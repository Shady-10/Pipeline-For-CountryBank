pipeline {
    agent any

    environment{
        regisrtyCred = 'ecr:us-east-1:awscred'
        repoURL = '228907518146.dkr.ecr.us-east-1.amazonaws.com/country-bank'
        countryBankRegistry = '228907518146.dkr.ecr.us-east-1.amazonaws.com'
    }

    stages {
        // First Stage (Fetching The Code From GitHub Repo)

        stage('Fetching The Code') {
            steps {
                git branch: 'master' , url: 'https://github.com/Shady-10/Pipeline-For-CountryBank.git'
            }
        }

        // Stage Two (Performing OWASP Analysis)

        stage('OWASP Analysis') {
            steps {
                dependencyCheck additionalArguments: '-s ./' , odcInstallation: 'DC'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }

        // Stage Three (Trivy Analysis)

        stage('Trivy') {
            steps {
                sh 'trivy fs .'
            }
        }
        // Stage Four (Building Docker Image)

        stage('Docker'){

            steps{

                script{

                    dockerImage = docker.build(repoURL + ":$BUILD_NUMBER" , ".")
                }
            }
        }

        // Stage Five (Uploading To ECR)

        stage('Uploading'){

            steps{

                script{

                    docker.withRegistry(countryBankRegistry,regisrtyCred){

                        dockerImage.push("$BUILD_NUMBER")
                        dockerImage.push("latest")
                    }
                }
            }
        }
    }
}
