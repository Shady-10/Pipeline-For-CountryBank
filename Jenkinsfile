pipeline{

    agent any 

    tools{
        jdk 'OracleJDK11'
    }

    stages{
        
        // First Stage (Fetching The Code From GitHub Repo)

        stage('Fetching The Code'){

            steps{

                git branch: 'master' , url: 'https://github.com/Shady-10/Pipeline-For-CountryBank.git'
            }
        }

        // Stage Two (Performing OWASP Analysis)

        stage('OWASP Analysis'){

            steps{

                dependencyCheck additionalArguments: '-s ./' , odcInstallation: 'DC'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }

        // Stage Three (Trivy Analysis)

        stage('Trivy'){

            steps{

                sh 'trivy fs .'
            }
        }

        
    }
}