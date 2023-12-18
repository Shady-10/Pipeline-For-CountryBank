pipeline {
    agent any

    stages {

        // Stage one (Fetching The Code From GITHUB)
        stage('Git Checkout') {
            steps {

                git branch: 'master' , url: 'https://github.com/Shady-10/Pipeline-For-CountryBank.git'

            }
        }
        
        // Stage two (Performing OWASP Analysis)
        stage('OWASP Dependency Check') {

            steps {

                 dependencyCheck additionalArguments: ' --s ./ ', odcInstallation: 'DC'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'

            }
        }
        
        // Stage three (Trivy Analysis)

        stage('Trivy') {

            steps {

                 sh "trivy fs ."

            }
        }
        
        // Stage four (Docker)

         stage('Build & deploy') {

            steps {

                 sh "docker-compose up -d"
                 
            }
        }
    }
}