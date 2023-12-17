pipeline {
    agent any

    tools {
        jdk 'OracleJDK11'
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


        stage('Docker'){

            steps{

                sh 'docker-compose up -d'
            }
        }
       // Stage Four (SonarQube Analysis)

        stage('SonarQube') {
            environment {
                scannerHome = tool 'SONAR4.7'
            }



            

            steps{

            script{

                    // Extracting The Jar File Terporirly

                    def tmpDir='/tmp/binary'
                    sh "mkdir -p ${tmpDir}"
                    sh "unzip -o /application/country_bank-1.0.jar -d ${tmpDir}"

                    withSonarQubeEnv('SONAR'){

                        sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=CountryBank \
                        -Dsonar.projectName=CountryBank \
                        -Dsonar.projectValue=1.0 \
                        -Dsonar.sources=src/ \
                        -Dsonar.java.binaries=${tmpDir}'''  // Adjust this path based on your Docker image'''
                    }
                }
            }
        }
    }
}
