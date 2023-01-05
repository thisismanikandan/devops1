pipeline{

    agent any

    stages {

        stage('Git Checkout'){

            steps{

                script{

                    git branch: 'main', url: 'https://github.com/thisismanikandan/devops1'
                }
            }
        }
        stage('UNIT testing'){

            steps{

                script{

                    sh 'mvn test'
                }
            }
        }
        stage('Integration testing'){

            steps{

                script{

                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
        stage('Maven build'){

            steps{

                script{

                    sh 'mvn clean install'
                }
            }
        }
        stage('Sonar Cube Analysis'){

            steps{

                script{

                    withSonarQubeEnv(credentialsId: 'sonarapikey') {

                        sh 'mvn clean package sonar:sonar'
                    }
                   }

                }
            }
            stage('Quality Gate Status'){

                steps{

                    script{

                        waitForQualityGate abortPipeline: false, credentialsId: 'sonarapikey'
                    }
                }
            }
        }

}
